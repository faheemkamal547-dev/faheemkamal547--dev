Spent the last while doing something I kept putting off: actually documenting my security lab work in one place instead of it living scattered across random folders on my laptop.

So here's what's on my GitHub profile now.

I ran a full Nessus scan against a deliberately vulnerable box and found 436 issues, 28 of them critical. Then I went further and actually exploited a few of them, a bind shell backdoor, an UnrealIRCd vuln, a weak VNC password, all the way to root, with proper remediation notes for each one. Finding a hole and not saying how to fix it feels half-finished.

I also ran a full ARP poisoning attack in a lab. Kali sitting in the middle of two Windows machines, watching their traffic pass through me in real time via Wireshark. Genuinely one of the more satisfying "oh, THAT'S how MITM works" moments I've had.

On the defensive side, I've got a Wazuh + Sysmon SOC pipeline running at home, and I traced an actual malware delivery to C2 sequence from a packet capture, start to finish.

Still very early in this journey, going through CEH at Corvit right now, but I wanted a profile that shows the actual work instead of just a list of buzzwords. Would genuinely appreciate it if a few of you took a look and told me what's missing or what a real Pentester would want to see more of.

Link's below 👇

#CyberSecurity #SOC #EthicalHacking #CEH #InfoSec #BlueTeam #RedTeam
