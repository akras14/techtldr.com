---
title: "One malformed WAP Push SMS crashes Android's phone process; Google says Won't Fix"
slug: "android-sms-of-death-wap-push-dos"
date: 2026-10-29T10:02:30+0000
summary: "A researcher disclosed a zero-click Android bug: a single malformed WAP Push SMS makes WapPushOverSms throw a NegativeArraySizeException and kill com.android.phone, dropping calls (including emergency calls). Google closed the report as Won't Fix / non-security after a 90-day timeline, there is no CVE, and a working proof-of-concept is public."
source: "https://smsofdeath.com/"
source_title: "SMS of Death: zero-click Android telephony DoS"
source_author: "Mark Blaszczyk"
source_site: "smsofdeath.com"
source_date: "2026-10-29"
hn_url: "https://news.ycombinator.com/item?id=50285452"
---

**Bottom line:** An independent researcher says a single crafted WAP Push SMS (port 2948) can crash `com.android.phone` on Android with no user interaction, and Google has declined to fix it, classing it as a non-security issue. There is no CVE. The researcher published the details and a working proof-of-concept on 29 Oct 2026 after the 90-day disclosure window.

**The bug.** In `WapPushOverSms.decodeWapPdu()`, the WSP header length is read as a uintvar into a `long`, cast to `int` with no range check, and used as a byte-array size. A 5-byte value `88 80 80 80 00` decodes to `0x80000000`, which becomes -2147483648 as a signed int. The `new byte[...]` then throws an uncaught `NegativeArraySizeException` on the telephony main thread. The message arrives as a data SMS, so the victim sees nothing and never learns the sender's number.

**Claimed impact:**
- Each message restarts the phone process, giving 1-2 seconds without service and dropping active calls, emergency calls included.
- Carriers re-send unacknowledged messages, which re-crashes the phone. In the researcher's testing, one message caused intermittent outages for up to a week.
- Sending about 30 messages in quick succession hit a race that corrupted the SMS SQLite database, after which the phone crashed on every boot. A factory reset only helped temporarily because the carrier redelivered the payload. Recovery meant moving the SIM to a non-Android device so the messages were acknowledged, then clearing the database over ADB. The author expects eSIM devices to be much harder to recover.
- Reported as reproducing 100% of the time on a Pixel 9 (April 2026 firmware) and several Samsung Galaxy and A-series phones.

**Why the author stresses Australia.** The author argues that Australian rules require carriers to block handsets that can't reach 000, so an attacker who can induce that state could get a victim's phone barred from the network.

**Proposed fix:** a bounds check before the cast (reject lengths below 0 or above the PDU length), which the author says was supplied to Google on the day of the first report.

**Disclosure history:** Reported 5 May 2026. Google closed it as Won't Fix (Infeasible) on 20 Jun, and closed a resubmission within minutes as Not Reproducible. Both were later marked duplicates. After three months of silence, the final Won't Fix / "Non-Security Issue" decision came on 29 Oct 2026.

These are the researcher's claims, and Google's side is only visible through the ticket quotes the author published.
