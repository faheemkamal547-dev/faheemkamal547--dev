<div align="center">

Header

[root@haider]─[~]$ whoami
> Cybersecurity student | Networking → Offensive Security → Detection Engineering
> Building a home SOC lab, one telemetry pipeline at a time


</div> <br>

~/about

role:          Aspiring SOC Analyst L1
current_cert:  CEH (Certified Ethical Hacker) — Corvit, NAVTTC
also_studying: CCNP, Huawei (GNS3 / eNSP)
focus:         Networking → Recon/Enum → Vuln Assessment → Exploitation → SOC Detection
philosophy:    Labs > theory. Evidence > claims. Every finding gets a remediation.


<br>

~/lab-infrastructure

Everything below runs in an isolated, host-only virtualized lab — Kali as the offensive box, Windows/Metasploitable2 as intentionally vulnerable targets, never against anything I don't own or have authorization for.

Layer Stack



Virtualization

VMware Workstation, EVE-NG, GNS3, Cisco Packet Tracer

Offensive box

Kali Linux

Targets

Metasploitable 2, Windows 10/11 VMs (XAMPP-simulated services)

SIEM / SOC

Wazuh (Manager, Indexer, Dashboard) + Windows Sysmon + Kali agent

Networking gear (virtual)

Cisco routers/switches (IOS), HSRP/VRRP/GLBP labs

<br>

~/skills

Networking
OS
Offensive
WebSec
SOC

Tools:

Nmap
Wireshark
Metasploit
Burp
Kali
Wazuh
Suricata
Ettercap

Nmap · Gobuster · Shodan · SearchSploit · Metasploit/Meterpreter · John the Ripper · Hydra · Nessus · OpenVAS/Greenbone · OWASP ZAP · Burp Suite · Wireshark · Ettercap · Lynis · Aircrack-ng

<br>

~/evidence-portfolio

Documented, reproducible lab work — not just "I installed the tool."

[+] Metasploitable 2 — Nessus Vulnerability Assessment
    436 findings | 28 Critical · 99 High · 148 Medium · 20 Low · 141 Info
    Critical: Apache PHP-CGI RCE, Shellshock, bind-shell backdoor,
              phpMyAdmin SQLi exposure, UnrealIRCd backdoor, weak VNC creds

[+] Metasploitable 2 — Controlled Exploitation & Pentest Report
    - Bind shell backdoor (ingreslock/1524)  → root shell, validated via Nmap + netcat
    - UnrealIRCd 3.2.8.1 backdoor            → Meterpreter session, root access
    - VNC weak credential ("password")       → validated via Metasploit aux scanner
    - Apache PHP-CGI argument injection      → vuln confirmed, alt. exploit path documented
    Each finding paired with remediation guidance.

[+] OWASP ZAP — Web Application Assessment (v2.17.0)
    Target: Metasploitable2 (DVWA, Mutillidae II, TWiki, phpMyAdmin, WebDAV)
    23 findings | Highest: High (MD5-crypt hash disclosure)
    Medium: missing CSP, directory browsing, vulnerable JS library, no anti-clickjacking

[+] Network Forensics — Malware Delivery + C2 Investigation (PCAP)
    Victim requested /update.exe x4 over HTTP:8000 → outbound TCP:4444 to C2 host
    ~566KB transferred in <1s post-handshake, ~67s sustained bidirectional session
    Pattern consistent with staged Meterpreter payload + interactive C2
    SOC flags: PE-over-HTTP, non-standard port, staged transfer, plaintext C2

[+] Nessus / Wireshark — Windows Network Scan Analysis
    15,621 packets / ~16 min capture | ~623 TCP ports probed via SYN scan
    SMB/RPC enumeration (LSA, SAMR, share enum) + default SNMP "public" string found
    Correctly distinguished recon/enum activity from actual exploitation

[+] Lynis — Linux Security Audit (Kali)
    269 tests | Hardening index: 60/100 | 1 warning, 49 suggestions
    Findings: inactive firewall/IDS, fail2ban gap, GRUB & PAM hardening opportunities

[+] Cisco 3640 — Nessus Vulnerability Assessment Report
    Network-device-focused scan with full severity breakdown

[+] OSINT Tooling
    Aliens Eye  — username OSINT scanner across 840+ platforms
    MailAccess  — email investigation/harvesting workflow (venv-based CLI tool)

[+] AI-Assisted Security Tooling
    HexStrike AI & Pentest-AI (PT-AI) — MCP-driven offensive tool orchestration,
    150+ modules, used strictly in authorized lab environments

[+] ARP Poisoning / MITM Lab — Ettercap + Wireshark
    Topology: Kali (attacker) · Windows 7 (victim 1) · Windows 10 (victim 2) — isolated host-only network

    Baseline:
    - Recorded IPv4 + MAC for both Windows hosts (ipconfig /all) and Kali's eth0 (ifconfig)
    - Verified clean ARP state on both victims (arp -a) — each held the other's true MAC, no Kali entry
    - Confirmed baseline reachability with ICMP between victims (no poisoning yet)

    Attack:
    - Launched Ettercap (GUI mode) on Kali, sniffing on eth0
    - Performed a host scan, identified both Windows machines from the host list
    - Assigned Win7 → Target 1, Win10 → Target 2
    - Enabled IP forwarding on Kali (net.ipv4.ip_forward = 1) so poisoned traffic would route
      through the attacker instead of black-holing
    - Launched MITM → ARP Poisoning attack

    Validation:
    - Re-checked arp -a on both victims — each host's ARP table now resolved the other's IP
      to Kali's MAC address, confirming successful cache poisoning
    - Started a live Wireshark capture on Kali's eth0
    - Generated ICMP traffic between the two Windows hosts and observed it transiting through
      the attacker, visible directly in the Wireshark capture — confirming the classic ARP
      poisoning outcome: two hosts believing they're talking directly to each other while all
      traffic is silently relayed (and inspectable) through the attacker in the middle

    Takeaway: demonstrates the core weakness ARP exploits — no authentication on ARP replies —
    and the practical mechanics of a Layer 2 MITM: poison → verify cache corruption → enable
    forwarding to stay transparent → intercept with a packet analyzer. Also reinforces the blue-team
    angle: this exact pattern (unsolicited/gratuitous ARP replies, duplicate MAC-to-IP mappings)
    is what ARP-spoofing detection rules in an IDS/SIEM are built to catch.


<br>

~/soc-detection-engineering

The part I'm actively pushing hardest on — going from "SIEM installed" to an actual detection lab.

Wazuh deployed: Manager / Indexer / Dashboard, with Windows + Kali agents reporting in

Sysmon configured on Windows, Microsoft-Windows-Sysmon/Operational feeding into Wazuh telemetry

Workflow: generate activity in the Kali lab → observe endpoint/network telemetry → tune Wazuh rules → trigger alerts → investigate

The PCAP/C2 case above is being used as a live detection-engineering exercise: correlate file download → process creation → outbound C2

Actively building toward: custom Wazuh rules/decoders, MITRE ATT&CK–mapped alerting, and repeatable attack→telemetry→alert→investigation runbooks (PowerShell abuse, persistence, credential access, C2).

<br>

~/cisco-networking

Hands-on Cisco IOS labs, not just theory:

IPv4/binary/subnetting · NAT (static + dynamic) · Standard & Extended ACLs · VLANs + inter-VLAN routing · RIP/OSPF/EIGRP/BGP concepts · HSRP/VRRP/GLBP failover labs · STP/BPDU & switching loops · DHCP · Wireless/WLC concepts

<br>

~/currently

+ Studying CEH at Corvit (24-day structured curriculum, hands-on 80%)
+ Learning CCNP + Huawei configuration (GNS3 / eNSP)
+ Maturing the Wazuh home SOC lab into a real detection pipeline
+ Preparing for SOC Analyst L1 roles


<br> <div align="center">

Everything above was performed in isolated, authorized lab environments — Metasploitable2, self-hosted VMs, and virtual network topologies. No unauthorized targets.

Footer

</div>
