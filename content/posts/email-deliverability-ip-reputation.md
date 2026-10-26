---
title: "Sending email is easy; inbox providers' IP-block reputation filters are the cost"
slug: "email-deliverability-ip-reputation"
date: 2026-10-26T21:02:55+0000
summary: "Evan Czaplicki, an ex-Gmail engineer, built email sending from scratch and found the protocol part is easy. What costs money is getting delivered: some inbox providers appear to judge senders by IP address block, so a shared cloud range with one spammer can sink your mail, and a clean reputation needs your own IP block and steady high volume."
source: "https://evancz.dev/why-is-it-so-expensive-to-send-an-email"
source_title: "Why is it so expensive to send an email?"
source_author: "Evan Czaplicki"
source_site: "evancz.dev"
source_date: "2026"
hn_url: "https://news.ycombinator.com/item?id=50242308"
---

**Bottom line:** Sending an email is technically about as hard as an HTTPS request. The price of "$15 per 1000 emails" comes from sender reputation, not from the protocol.

- **The protocol is simple.** SMTP is a plaintext back-and-forth over TCP (originally seven round trips, no encryption). Modern sending adds TLS and DNS records (TXT records with public keys) so receivers can verify the sender's domain, since the original protocol lets anyone claim any sender.
- **Delivery is the hard part.** After setting this up in Haskell and C, the author's mail reached Gmail but did not even land in spam folders at Outlook. Each provider has its own opaque reputation system: some judge the domain, some the IP address, and some appear to judge the whole IP address block.
- **Why that costs money.** If inboxes penalize whole blocks, then a small cloud provider's shared range can be blacklisted because of another customer's spam. The author's conclusion is that reliable delivery means having your own IP block and sending enough volume through it daily to build trust. Email vendors sell that reputation.
- **Caveat.** The author says he has no inside knowledge of how providers filter, that this is his personal inference, and that Google evidently doesn't rely as heavily on block-level blacklists, so it may not have to be this way.
