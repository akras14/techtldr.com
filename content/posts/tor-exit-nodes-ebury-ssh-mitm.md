---
title: "333 Tor exit relays, about 10% of exits, ran an Ebury-backed SSH MITM"
slug: "tor-exit-nodes-ebury-ssh-mitm"
date: 2026-10-20T15:02:54+0000
summary: "On Oct 2, 333 Tor exit relays infected with the Ebury 1.8.3 OpenSSH backdoor began redirecting port-22 traffic to 12 interception servers that stole passwords and infected the destination servers. The author spotted a host-key warning, scanned every exit with exitmap, and the Tor team flagged them BadExit by Oct 4."
source: "https://kaba.cx/posts/20261020_Tor/"
source_title: "How I got over 300 Tor exit nodes removed"
source_author: "KABA"
source_site: "kaba.cx"
source_date: "2026-10-20"
hn_url: "https://news.ycombinator.com/item?id=50166257"
---

**Bottom line:** Attackers who had owned the relay servers since August turned 333 of roughly 3,000 Tor exits into an SSH man-in-the-middle. A single "REMOTE HOST IDENTIFICATION HAS CHANGED" warning led the author to find them, and the Tor network health team shut them down within days.

- **Attack.** Infected Linux hosts running Ebury 1.8.3 (an OpenSSH backdoor and credential stealer) had torrc edited to become exits. Port-22 traffic was redirected to 12 interception servers. If you ignored the warning, the attacker stole the password, watched your commands, and opened a second connection to infect your server with Ebury. The relays also blocked ports so they only handled unencrypted mail traffic.
- **Detection.** Using exitmap, the author pinned each exit, connected from server A to server B, and compared host-key fingerprints and source IPs. About a dozen served fake fingerprints; over 300 only redirected, which was enough to confirm infection. A sshesame honeypot showed the attackers used paramiko and a base64-encoded Perl installer.
- **Timeline.** Oct 2 start; Oct 4 operators warned and relays marked BadExit; Oct 5 attackers restarted some as middle relays, then everything went down. Exit capacity has not recovered.
- **Unknowns.** How the servers were compromised (a hosting-company breach is one theory) and who is behind it; Ebury suggests a financially motivated group.
- **Takeaway.** Use SSH keys instead of passwords, and treat relays as highly targeted production machines.
