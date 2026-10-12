---
title: "Flask cookie signed with its own OAuth secret gave RCE on seven Google domains"
slug: "google-devsite-flask-cookie-rce"
date: 2026-10-12T16:03:46+0000
summary: "A researcher got a $90,000 bounty for remote code execution on Google's shared DevSite platform. The Flask session cookie on developers.google.com carried a pickled OAuth flow object containing Google's OAuth client_secret in plaintext, and that same secret was the Flask SECRET_KEY used to sign the cookie. Anyone could forge a valid session, and the OAuth callback unpickled it."
source: "https://simonkoeck.com/writeups/google-devsite-flask-session-pickle-rce"
source_title: "How I Found a $90,000 Remote Code Execution Vulnerability in Google"
source_author: "Simon Koeck"
source_site: "simonkoeck.com"
source_date: "2026-10-12"
hn_url: "https://news.ycombinator.com/item?id=50052638"
---

**Bottom line:** The Flask session cookie issued by Google's OAuth flow on developers.google.com stored a pickled `OAuth2WebServerFlow` object, including the OAuth `client_secret`. That secret was also the app's Flask `SECRET_KEY`, so anyone could read it from their own cookie and sign arbitrary sessions. The OAuth callback calls `pickle.loads` on the flow stored in the session, which turns a forged cookie into unauthenticated remote code execution. Google paid $90,000 (a $75,000 base times 1.2 for report quality) and fixed it on Oct 7, 2026.

**How it worked**
- Flask session cookies are signed, not encrypted. The roughly 1,200-byte cookie decoded to JSON with a base64 "bytes" value, which was a pickle containing the client ID and secret.
- The author recomputed the itsdangerous HMAC signature offline using the client secret as the key and matched the server's signature exactly. Flipping a signature byte was rejected; re-signing with the recovered secret was accepted.
- Pickle's `__reduce__` lets a payload name any callable. Rather than run commands on production, the author proved execution with `time.sleep(N)`, and response times tracked N (0.2s, 3.2s, 5.2s, 8.3s). Controls (no cookie, bad signature, missing flow entry) were all rejected.
- Because DevSite is a shared platform, the same key signed valid sessions on firebase.google.com, developer.android.com, source.android.com and docs.partner.android.com.

**Fix recommendations:** rotate the exposed secrets, use a separate high-entropy `SECRET_KEY` unrelated to any OAuth credential, keep the OAuth flow server-side behind an opaque reference, and never use pickle across a client boundary. Any one of these would have broken the chain.

Report submitted Aug 6, triaged Aug 7, reward decided Aug 27, fixed Oct 7.
