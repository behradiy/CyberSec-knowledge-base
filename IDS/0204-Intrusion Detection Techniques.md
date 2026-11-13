## Teaching the Watchdog to Bark: An Intrusion Detection Techniques Roadmap

*Part 4 of the [IDS developer roadmap](02-what%20should%20you%20learn.md) · ← [0203-Programming for IDS](0203-Programming%20for%20IDS.md) · [0205-Operating System Internals](0205-Operating%20System%20Internals.md) → · Resources last checked: November 2026*

This is the heart of the whole roadmap: how an IDS actually decides that something is an attack. You'll learn the classic approaches (signatures and anomalies), how to write real detection rules, how professionals manage those rules like software, and the most underrated skill of all: keeping false alarms low enough that people still listen when the watchdog barks.

**Phase 1: Detection Fundamentals and Metrics (1-2 Months):**

* **Signature-based detection:** match known attack patterns. Fast and precise, but blind to anything new.
* **Anomaly-based detection:** learn what "normal" looks like and flag deviations. It can catch new attacks, but it's noisy.
* **Hybrid detection:** combine both, as most real systems do.
* **What you can monitor:** full packet capture, flow records, transaction logs (like Zeek's) and alerts. Each trades detail for storage.
* **Measuring a detector:** precision, recall, F1 and false-positive rate, and the **base-rate fallacy**. Attacks are rare, so even a 0.1% false-positive rate on millions of daily events means thousands of false alarms. Axelsson's classic paper (below) is a must-read.

**Phase 2: Network Signatures and Protocol Analysis (2-3 Months):**

* **Rule anatomy:** every Snort or Suricata rule has an action, a header (protocol, addresses, ports, direction) and options. Learn *sticky buffers* (matching inside a specific protocol field) and `fast_pattern`.
* **Suricata 8 features:**
    * **Transactional rules** (`=>`) match a request and its response in one rule.
    * The **`absent`** keyword matches when a field is missing.
    * `requires` makes a rule load only on engines that support it.
    * **Firewall mode** (default-drop) is still experimental.
* **Stateful detection:** flowbits and xbits connect events across packets and flows, and datasets hold large indicator lists.
* **Protocol analysis with Zeek:** script on network events and use `conn.log` fields such as `conn_state` and `history`. Zeek's Notice, Intel and SumStats frameworks handle alerting, threat-intel matching and statistics.
* **Simple anomaly detection:** baselines with percentiles and time-of-day patterns, and spotting **beaconing** (regular intervals, consistent sizes, long-lived connections).
* **Test offline:** run `suricata -r` and `zeek -r` against capture files before anything goes live.

**Phase 3: Detection-as-Code (2-3 Months):**

Modern teams manage detections like software: version control, tests and reviews.

* **Sigma:** a generic, open format for log-based detection rules, converted to many SIEM query languages with pySigma and `sigma-cli`. The specification is at version 2.1.
* **YARA-X:** the successor to YARA for matching patterns in files and content. YARA itself is now in maintenance mode, and some rules need small changes to work in YARA-X.
* **Testing in CI:** every rule should have samples it *must* catch and samples it must *not* catch, plus linting and peer review.
* **Mapping and robustness:** map detections to MITRE ATT&CK, judge how hard each one is to evade with MITRE's *Summiting the Pyramid*, and document each one using Palantir's *Alerting and Detection Strategy* (ADS) template.

**Phase 4: Tuning and Operating at Scale (1-2 Months, then Ongoing):**

* **Root-cause false positives:** most are legitimate-but-unusual activity, not bugs.
* **Tuning tools:** allowlists, thresholds, suppression and enrichment (adding context such as asset owner or threat intel).
* **Measure what matters:** precision per rule, alerts per analyst per day, and regular validation by replaying attacks or emulating adversaries.
* **Maturity:** use the Detection Engineering Maturity Matrix to see where your team stands.

**Resources:**

* **Free Online Courses and Documentation:**
    * [Suricata 8 User Guide – Rules](https://docs.suricata.io/en/suricata-8.0.8/rules/intro.html) (official, free; includes [payload keywords such as `absent`](https://docs.suricata.io/en/suricata-8.0.8/rules/payload-keywords.html) and the [firewall mode design](https://docs.suricata.io/en/suricata-8.0.8/firewall/firewall-design.html))
    * [The Book of Zeek](https://docs.zeek.org/en/current/) and the [conn.log reference](https://docs.zeek.org/en/current/logs/conn.html)
    * [Sigma – Getting Started](https://sigmahq.io/docs/guide/getting-started.html) and the [Sigma specification](https://github.com/SigmaHQ/sigma-specification)
    * [YARA-X – Differences with YARA](https://virustotal.github.io/yara-x/docs/writing_rules/differences-with-yara/)
    * [MITRE ATT&CK Training](https://attack.mitre.org/resources/learn-more-about-attack/training/) (including "TTP-Based Threat Hunting and Detection Engineering", free)
    * [NIST SP 800-94 – Guide to Intrusion Detection and Prevention Systems](https://csrc.nist.gov/pubs/sp/800/94/final) (2007, still the current NIST guide)
* **Free Frameworks:**
    * [Palantir Alerting and Detection Strategy framework](https://github.com/palantir/alerting-detection-strategy-framework)
    * [Summiting the Pyramid (MITRE Center for Threat-Informed Defense)](https://ctid.mitre.org/projects/summiting-the-pyramid/)
    * [Detection Engineering Maturity Matrix](https://detectionengineering.io/)
* **Books:**
    * **Practical Threat Detection Engineering by Megan Roddie, Jason Deyalsingh and Gary J. Katz** (Packt, 2023): planning, building and validating detections, end to end.
    * **The Practice of Network Security Monitoring by Richard Bejtlich** (No Starch Press, 2013): the method behind collection, detection and analysis. [Publisher page](https://nostarch.com/nsm)
    * **Applied Network Security Monitoring by Chris Sanders and Jason Smith** (Syngress, 2013): hands-on collection, detection and analysis, including Snort/Suricata rule writing. [Publisher page](https://shop.elsevier.com/books/applied-network-security-monitoring/sanders/978-0-12-417208-1)
    * **Network Security Through Data Analysis, 2nd Edition by Michael Collins** (O'Reilly, 2017): statistics and exploratory analysis for anomaly detection.
    * **Intrusion Detection by Rebecca Gurley Bace** (Macmillan Technical Publishing, 2000): a historical classic, for understanding where the field came from.
* **Must-Read Papers:**
    * Dorothy E. Denning, [An Intrusion-Detection Model](https://doi.org/10.1109/TSE.1987.232894) (IEEE Transactions on Software Engineering, 1987): where it all started.
    * Vern Paxson, [Bro: A System for Detecting Network Intruders in Real-Time](https://doi.org/10.1016/S1389-1286(99)00112-7) (Computer Networks, 1999): the design behind Zeek (formerly Bro).
    * Martin Roesch, [Snort – Lightweight Intrusion Detection for Networks](https://www.usenix.org/legacy/event/lisa99/roesch.html) (USENIX LISA, 1999): the original Snort paper.
    * Stefan Axelsson, [The Base-Rate Fallacy and the Difficulty of Intrusion Detection](https://doi.org/10.1145/357830.357849) (ACM TISSEC, 2000): why false-positive rates matter more than you think.
    * Alahmadi, Axon and Martinovic, [99% False Positives: A Qualitative Study of SOC Analysts' Perspectives on Security Alarms](https://www.usenix.org/conference/usenixsecurity22/presentation/alahmadi) (USENIX Security, 2022): what false alarms do to the humans who receive them.
* **Practice:**
    * [Malware-Traffic-Analysis.net exercises](https://www.malware-traffic-analysis.net/training-exercises.html): write Suricata rules and Zeek scripts that catch the malicious traffic, then run them on normal traffic to check for false positives (live malware inside: use an isolated VM)
    * [Emerging Threats Open ruleset](https://rules.emergingthreats.net/open/) (free; currently published for Suricata 7.0.3 and newer, and generally loads on 8): run it against captures and tune the noisy rules
    * [SigmaHQ rules](https://github.com/SigmaHQ/sigma) with [sigma-cli](https://github.com/SigmaHQ/sigma-cli): convert rules for your SIEM, and contribute a rule or a false-positive fix
    * Study how professionals test detections as code: [Splunk Security Content](https://research.splunk.com/) and [Elastic detection rules](https://github.com/elastic/detection-rules) (free to read; Elastic's license is source-available, not open source)
    * [RITA](https://github.com/activecm/rita): compare its beacon findings with your own statistics on the same Zeek logs
    * [CyberDefenders blue-team challenges](https://cyberdefenders.org/blueteam-ctf-challenges/) (labs without the "Premium" tag are free)

**Beyond the Roadmap:**

* **Write one rule a week:** pick a recent threat report and turn one behavior into a tested detection.
* **Contribute upstream:** SigmaHQ, Emerging Threats and Zeek packages all accept community contributions, and it looks great on a CV.
* **Respect the analyst:** a detection that fires constantly is worse than no detection. Tune ruthlessly.

**Wishing you the best of luck!** Rules need data, and much of that data comes from inside the operating system, which is next in [0205-Operating System Internals](0205-Operating%20System%20Internals.md).
