## Know Your Enemy: A Security Concepts Roadmap for Aspiring IDS Developers

*Part 2 of the [IDS developer roadmap](02-what%20should%20you%20learn.md) · ← [0201-Network Fundamentals](0201-Network%20Fundamentals.md) · [0203-Programming for IDS](0203-Programming%20for%20IDS.md) → · Resources last checked: November 2026*

An IDS is only as good as its understanding of the enemy. Network fundamentals taught you how normal traffic looks; this roadmap teaches you how attacks look, how defenders describe them, and how to decide which threats matter most. Think of it as studying the criminal's playbook before you design the alarm system.

**Phase 1: The Threat Landscape and Attack Vectors (1-2 Months):**

Start with the attacks your IDS will actually face:

* **Core ideas:** the CIA triad (confidentiality, integrity, availability) and the idea of an *attack surface*.
* **Malware and delivery:** how malware families are delivered (phishing, malicious downloads, supply-chain compromise) and what they do after landing.
* **Denial of service:** volumetric floods, protocol abuse (such as SYN floods) and application-layer attacks.
* **Web attacks:** SQL injection, cross-site scripting (XSS), server-side request forgery (SSRF) and request smuggling. Many network alerts are about these.
* **Credential abuse and lateral movement:** stolen passwords, pass-the-hash, and attackers hopping between machines using remote services. This is where internal network monitoring shines.

**Phase 2: Speaking the Defender's Language: Adversary Frameworks (1-2 Months):**

Frameworks give you a shared vocabulary for describing attacks and measuring your coverage:

* **The Cyber Kill Chain:** Lockheed Martin's seven steps of an intrusion, from reconnaissance to actions on objectives, and the more detailed *Unified Kill Chain*.
* **MITRE ATT&CK:** the industry-standard catalog of attacker tactics and techniques. Since version 18, ATT&CK describes detection through *Detection Strategies and Analytics*, and version 19 split the old "Defense Evasion" tactic into **Stealth** and **Defense Impairment**. The current release is v20.1 (October 2026).
* **Coverage mapping:** using ATT&CK Navigator to color in which techniques your detections cover, and where your blind spots are.

**Phase 3: Threat Modeling and the Vulnerability Ecosystem (~1 Month):**

Now learn to think systematically about what can go wrong, and how the world tracks vulnerabilities:

* **Threat modeling:** the four key questions (What are we working on? What can go wrong? What are we going to do about it? Did we do a good job?), STRIDE, data-flow diagrams and trust boundaries.
* **CVE and CWE:** how individual vulnerabilities (CVE) and weakness types (CWE) are named.
* **CVSS v4.0:** how severity is scored (Base, Threat, Environmental and Supplemental metrics).
* **EPSS:** a daily probability that a vulnerability will be exploited in the next 30 days.
* **CISA KEV:** the catalog of vulnerabilities *known* to be exploited in the wild. Combining KEV, EPSS and CVSS is the modern way to decide which signatures to write first.

**Phase 4: The Attacker Mindset and Adversary Emulation (1-2 Months, then Ongoing):**

The best detection engineers think like attackers:

* **Adversary emulation:** safely running real attack techniques in a lab to generate the telemetry your IDS should catch.
* **Purple teaming:** attack, check what was detected, improve the detection, repeat.
* **Capture the Flag (CTF):** competitions that build intuition for how attacks really work.

**Resources:**

* **Free Online Courses:**
    * [MITRE ATT&CK Training](https://attack.mitre.org/resources/learn-more-about-attack/training/) (official ATT&CK Fundamentals and Cyber Threat Intelligence training: videos, slides and exercises, no sign-up)
    * [Professor Messer's CompTIA Security+ SY0-701 Course](https://www.professormesser.com/security-plus/sy0-701/sy0-701-video/sy0-701-comptia-security-plus-course/) (free videos covering all security fundamentals; note that CompTIA launches the new SY0-801 exam in November 2026)
    * [Cisco Networking Academy – Introduction to Cybersecurity](https://www.netacad.com/courses/introduction-to-cybersecurity) (free, about 6 hours, self-paced with labs)
    * [MIT OpenCourseWare 6.858 – Computer Systems Security](https://ocw.mit.edu/courses/6-858-computer-systems-security-fall-2014/) (free university course with lecture videos)
* **Free References:**
    * [The Cyber Kill Chain (Lockheed Martin)](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html)
    * [CVSS v4.0 specification and calculator (FIRST)](https://www.first.org/cvss/v4-0/) · [EPSS (FIRST)](https://www.first.org/epss/) · [CISA Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)
    * [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) · [Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org/)
* **Books:**
    * **Security Engineering, 3rd Edition by Ross Anderson** (Wiley, 2020): the classic, encyclopedic guide to building dependable systems. The author made every chapter [freely available as PDF](https://www.cl.cam.ac.uk/archive/rja14/book.html).
    * **Foundations of Cybersecurity, 2nd Edition by Jason Andress** (No Starch Press, 2026): a beginner-friendly tour of security fundamentals. [Publisher page](https://nostarch.com/foundationsinfosec)
    * **Threat Modeling: Designing for Security by Adam Shostack** (Wiley, 2014): the standard book on threat modeling. A second edition is announced for February 2027. [Author page](https://shostack.org/books/threat-modeling-book)
    * **Intelligence-Driven Incident Response, 2nd Edition by Rebekah Brown and Scott J. Roberts** (O'Reilly, 2023): how to use threat intelligence and the kill chain in detection and response.
    * **The Practice of Network Security Monitoring by Richard Bejtlich** (No Starch Press, 2013): where security concepts meet network monitoring. [Publisher page](https://nostarch.com/nsm)
* **Practice:**
    * [TryHackMe – Cyber Kill Chain](https://tryhackme.com/room/cyberkillchainzmt) and [Unified Kill Chain](https://tryhackme.com/room/unifiedkillchain) (free rooms, free account needed)
    * [ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/) (free, in the browser: build your first coverage map)
    * [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team) (free, open-source tests mapped to ATT&CK, to run in a lab and see what your IDS catches)
    * [PortSwigger Web Security Academy](https://portswigger.net/web-security) (free web-attack labs) and [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) (a deliberately vulnerable app to practice on)
    * [CyLab Security Academy](https://cylabacademy.org) (Carnegie Mellon's free CTF platform, formerly picoCTF)

**Beyond the Roadmap:**

* **Read threat reports:** pick one public threat-intelligence report a week and map its techniques in ATT&CK Navigator. The [How attackers use AI](../AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md) article in this repo links several good ones.
* **Follow KEV:** subscribe to CISA's KEV updates and ask yourself, "Would my IDS see this being exploited?"
* **Stay legal:** practice only in your own lab or on platforms built for it.

**Wishing you the best of luck** on the next step of your journey! Once you can describe attacks in the defender's language, you're ready to start building the tools that catch them, which is where [0203-Programming for IDS](0203-Programming%20for%20IDS.md) comes in.
