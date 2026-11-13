## The Detective's Toolkit: A Network Security Tools Roadmap for Aspiring IDS Developers

*Part 6 of the [IDS developer roadmap](02-what%20should%20you%20learn.md) · ← [0205-Operating System Internals](0205-Operating%20System%20Internals.md) · [0207-Cryptography for IDS Developers](0207-Cryptography%20for%20IDS%20Developers.md) → · Resources and versions last checked: November 2026*

Every great detective has a favorite magnifying glass. For network defenders, that toolkit is a set of mostly free, open-source tools that capture, dissect and analyze traffic. Learning them deeply does two things: it makes you a better analyst, and it shows you how mature IDS engines are designed, which is exactly what you want to learn before building your own.

**Phase 1: Packet Analysis (1-2 Months):**

* **Wireshark:** dissect traffic layer by layer, follow TCP streams and master *display filters*.
* **tcpdump and tshark:** capture and filter traffic from the command line, using *BPF capture filters* (a different syntax from display filters, and a classic source of confusion).
* **Nmap:** see your network the way an attacker does during reconnaissance, and recognize what scans look like in your captures.

**Phase 2: Network Security Monitoring Sensors and IDS Engines (2-3 Months):**

* **Zeek:** turns raw traffic into rich, structured logs (connections, DNS, HTTP, TLS, files) and can be scripted to detect almost anything.
* **Suricata:** a high-performance signature IDS/IPS that writes alerts and metadata as `eve.json`.
* **Snort 3:** the classic signature IDS, rebuilt. Use Snort 3 only: Cisco Talos ended support for all open-source Snort 2 versions in October 2026.
* **Full packet capture with Arkime:** store and search complete traffic, so you can go back in time when an alert fires.

**Phase 3: Platforms, Threat Hunting and Forensics (1-2 Months):**

* **Security Onion:** a free platform that bundles Zeek, Suricata, full packet capture and a web interface. Install the current version 3; version 2.4 reached end of life in October 2026.
* **RITA:** analyzes Zeek logs to find *beaconing*, the regular "phone home" pattern of malware talking to its command server.
* **NetworkMiner:** extracts files, images, credentials and hosts from captures, which is great for forensics.
* **Case work:** take a malicious capture and write up what happened, from first contact to data theft.

**Current Versions (November 2026):**

| Tool | Version | Note |
|---|---|---|
| [Wireshark](https://www.wireshark.org/download.html) | 4.6.x | |
| [tcpdump / libpcap](https://www.tcpdump.org/) | 4.99.7 / 1.10.7 | |
| [Zeek](https://zeek.org/get-zeek/) | 9.0.x (LTS) | 8.0 is the previous LTS |
| [Suricata](https://suricata.io/download/) | 8.0.x | Suricata 7 reached end of life in July 2026 |
| [Snort](https://www.snort.org/downloads) | 3.x | Snort 2 is no longer supported |
| [Security Onion](https://docs.securityonion.net/en/3/main/) | 3.x | 2.4 reached end of life in October 2026 |
| [Arkime](https://arkime.com/) | 6.x | 7.0 is in release-candidate testing |
| [Nmap](https://nmap.org/changelog.html) | 7.99x | |
| [RITA](https://github.com/activecm/rita/releases) | 5.x | Reads Zeek logs |
| [NetworkMiner](https://www.netresec.com/?page=NetworkMiner) | 3.x | Free edition available |

**Resources:**

* **Free Online Courses and Documentation:**
    * [Security Onion Essentials](https://securityonionsolutions.com/training) (free on-demand course, about 2 hours) and the [Security Onion documentation](https://docs.securityonion.net/en/3/main/)
    * [Wireshark User's Guide](https://www.wireshark.org/docs/wsug_html_chunked/) and the [Wireshark Learn page](https://www.wireshark.org/learn) (free)
    * [SharkFest Retrospective](https://sharkfest.wireshark.org/retrospective/) (free slides and recordings from every Wireshark conference since 2008)
    * [The Book of Zeek](https://docs.zeek.org/en/current/) and the [Suricata User Guide](https://docs.suricata.io/en/suricata-8.0.8/) (free, official)
    * [Snort 3 Rule Writing Guide](https://docs.snort.org/) (free)
    * [Nmap Network Scanning, online edition](https://nmap.org/book/) (about half the book is free online)
* **Books:**
    * **Practical Packet Analysis, 3rd Edition by Chris Sanders** (No Starch Press, 2017): the best hands-on introduction to Wireshark. [Publisher page](https://nostarch.com/packetanalysis3)
    * **The Practice of Network Security Monitoring by Richard Bejtlich** (No Starch Press, 2013): the timeless method behind NSM. The tools have changed, but the ideas haven't. [Publisher page](https://nostarch.com/nsm)
    * **Applied Network Security Monitoring by Chris Sanders and Jason Smith** (Syngress, 2013): collection, detection and analysis in practice. [Publisher page](https://shop.elsevier.com/books/applied-network-security-monitoring/sanders/978-0-12-417208-1)
    * **Nmap Network Scanning by Gordon "Fyodor" Lyon** (Nmap Project, 2009): the official Nmap guide, by its creator. [Book site](https://nmap.org/book/)
* **Practice:**
    * [Malware-Traffic-Analysis.net exercises](https://www.malware-traffic-analysis.net/training-exercises.html) (free, real malicious traffic with answers, updated constantly. The zips contain live malware, so only open them in an isolated VM)
    * Public capture collections: [Netresec's list of PCAP files](https://www.netresec.com/?page=PcapFiles) and [Wireshark Sample Captures](https://wiki.wireshark.org/SampleCaptures)
    * **Home lab:** install Security Onion 3 in a virtual machine, replay captures into it, and add RITA to hunt for beacons.
    * [Blue Team Labs Online](https://blueteamlabs.online/) (free challenges; investigations need a paid plan) and [CyberDefenders](https://cyberdefenders.org/labs) (some free labs, including network forensics)

**Beyond the Roadmap:**

* **Read the engines' rules:** browse the free Emerging Threats Open ruleset to see how professionals write signatures. Rule writing itself is covered in [0204-Intrusion Detection Techniques](0204-Intrusion%20Detection%20Techniques.md).
* **Contribute:** Suricata, Zeek and Wireshark all welcome new protocol parsers and bug reports.
* **Stay on supported versions:** end-of-life engines stop getting rule updates and security fixes. Check the release pages above at least twice a year.

**Wishing you the best of luck!** Many of the protocols these tools inspect are now encrypted, which is why the next step is [0207-Cryptography for IDS Developers](0207-Cryptography%20for%20IDS%20Developers.md).
