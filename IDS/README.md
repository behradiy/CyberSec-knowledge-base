# Intrusion Detection Systems (IDS)

The `IDS` folder focuses on Intrusion Detection Systems (IDS), critical components of any security infrastructure. IDS monitor network traffic for suspicious activities that may indicate a potential attack.

**Content**

* Detection methodologies - Explanation of common techniques used by IDS to detect intrusions, such as signature-based and anomaly-based detection.
* System types - Overview of different types of IDS, including network-based IDS (NIDS) and host-based IDS (HIDS).
* Configuration best practices - Guidelines for configuring your IDS for optimal effectiveness.
* Additional resources - Links to relevant articles, documentation, and vendor websites.

**Articles (read in order)**

* [01-what is that](01-what%20is%20that.md) - What an IDS is, NIDS vs. HIDS, and their limits.
* [02-what should you learn](02-what%20should%20you%20learn.md) - The knowledge you need to become an IDS developer, and the full 8-step roadmap at a glance.
  * [0201-Network Fundamentals](0201-Network%20Fundamentals.md) - TCP/IP, traffic analysis and Wireshark.
  * [0202-Security Concepts](0202-Security%20Concepts.md) - Attack vectors, MITRE ATT&CK, threat modeling, CVE/CVSS/EPSS/KEV.
  * [0203-Programming for IDS](0203-Programming%20for%20IDS.md) - Python, C, Rust, Suricata Lua, Zeek scripting and Go.
  * [0204-Intrusion Detection Techniques](0204-Intrusion%20Detection%20Techniques.md) - Signatures, anomalies, detection-as-code (Sigma, YARA-X), metrics and tuning.
  * [0205-Operating System Internals](0205-Operating%20System%20Internals.md) - Linux, eBPF, Windows ETW and Sysmon, and host-based IDS tools.
  * [0206-Network Security Tools](0206-Network%20Security%20Tools.md) - Wireshark, Zeek, Suricata, Snort 3, Security Onion and current versions.
  * [0207-Cryptography for IDS Developers](0207-Cryptography%20for%20IDS%20Developers.md) - TLS 1.3, QUIC, JA4 fingerprints, Encrypted Client Hello and post-quantum.
  * [0208-Machine Learning for IDS](0208-Machine%20Learning%20for%20IDS.md) - ML baselines, datasets and evaluating detectors honestly.
* [03-AI meets network detection](03-AI%20meets%20network%20detection.md) - *New in the 2026 update:* why "98% accurate" AI traffic models were mostly shortcut learning, the state of IDS datasets, whether LLMs can detect intrusions, and what changed in Suricata, Zeek, Snort and host-based detection.
* [04-The agentic SOC](04-The%20agentic%20SOC.md) - *New in the 2026 update:* AI agents that triage alerts and investigate incidents, what the vendors shipped, and what the evidence and benchmarks actually show.

Related paper review: [Papers/06 - The Sweet Danger of Sugar](../Papers/06-%20The%20Sweet%20Danger%20of%20Sugar%20-%20Debunking%20Representation%20Learning%20for%20Encrypted%20Traffic%20Classification%20%28July%202025%29.md).

**Understanding and Implementing IDS**

This folder helps you grasp the core principles of IDS and guides you in implementing them within your network. A well-configured IDS can significantly enhance your security posture. Just keep following the readme files from top to bottom, all the needed resources are linked vividly, also feel free to [DM](https://t.me/B3hrady) me for extra information or just say hello or something :smile:

**Contribute your IDS expertise! We welcome information on new IDS techniques or configuration recommendations.**
