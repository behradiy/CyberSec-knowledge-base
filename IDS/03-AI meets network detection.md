## AI Meets Network Detection: The Hype, the Reality Check, and What Actually Changed (2024–2026)

*Covers July 2024 – November 2026.*

In 2024 the promise was simple: take the transformer architecture that powers ChatGPT, pretrain it on mountains of network traffic, and get a digital watchdog that understands packets like a language. Papers reported 97–99% accuracy. Between 2025 and 2026, careful researchers checked the watchdog's homework, and much of it turned out to be copied from the answer key.

**The Big Reality Check: Shortcut Learning**

"Shortcut learning" means a model finds an easy, meaningless clue that happens to correlate with the label (an IP address, a TCP sequence number, a timestamp) instead of learning the actual behavior. It scores brilliantly in the lab and fails in the real world.

* **SoK: Decoding the Enigma of Encrypted Network Traffic Classifiers (IEEE S&P 2025):** The popular "encrypted traffic" datasets turned out to be mostly *not* encrypted: ISCXVPN2016 is 98.9% unencrypted, USTC-TFC2016 94.7%, ISCXTor2016 89.3%. After randomizing the features that leak labels, ET-BERT dropped from 0.91 to 0.59 and YaTC from 0.93 to 0.68. The authors released CipherSpectrum, a TLS 1.3 dataset, as a fix.
* **The Sweet Danger of Sugar (ACM SIGCOMM 2025):** The knockout punch. With a proper per-flow split and a frozen encoder, ET-BERT and YaTC fell from about 97–98% to 11–16%. A *randomly initialized* ET-BERT scored about the same as the pretrained one, so the pretraining wasn't doing the work. Full review in [Papers/06](../Papers/06-%20The%20Sweet%20Danger%20of%20Sugar%20-%20Debunking%20Representation%20Learning%20for%20Encrypted%20Traffic%20Classification%20%28July%202025%29.md).
* **netFound v5 (May 2026):** The netFound team pushed back with a "principled design" for network foundation models. It uses a protocol-aware tokenizer, excludes payloads and IP addresses so the model can't cheat on them, and is pretrained on about 4 billion campus flows (53M to 663M parameters). It is a constructive answer, but still a preprint, and its own results show it losing on at least one benchmark.

**Lessons for anyone building an ML-based IDS:**

* **Split by flow or by time, never by packet:** if packets from the same connection land in both training and test sets, the model can memorize connections.
* **Hunt for leaky features:** IPs, ports, sequence numbers and timestamps often encode the label by accident.
* **Test on a different network or time period:** a model that only works on the network it was trained on is not detecting attacks, it is recognizing a lab.
* **Always compare with a simple baseline:** in the Sugar paper, LightGBM on hand-picked header fields beat every deep model, and ran up to ~2,000× faster.

**Datasets: The Problem Moved from Labels to Design**

* **Label errors are old news:** mislabeled flows in CIC-IDS2017 and CSE-CIC-IDS2018 were documented in 2021–2023 (Engelen et al. and others).
* **Bad Design Smells (IEEE EuroS&P 2024):** an audit of 7 heavily cited NIDS datasets found 6 recurring "data design smells" that bias experiments. They go hand in hand with poor data diversity, unclear labeling and loosely defined generalization goals.
* **A survey of 89 datasets (Computers & Security, 2025):** Goldschmidt and Chudá compared every public NIDS dataset they could find on 13 properties. Useful when choosing one.
* **NetFlow v3 (2025):** adds 10 time-based features (53 instead of 43) to UNSW-NB15, ToN-IoT, BoT-IoT and CSE-CIC-IDS2018. It is rebuilt from the same original captures, so it inherits their design flaws.
* **Bottom line:** As of late 2026 there is still no widely adopted, cleanly designed, modern labeled NIDS benchmark. That gap is an opportunity for researchers.

**Can an LLM Be Your IDS? Mostly No (But It's a Great Explainer)**

* **From Flows to Words (October 2025):** on UNSW-NB15, zero-shot Mistral-7B reached 0.09 accuracy. The best guided setup (Qwen2.5-7B with domain hints) reached 0.785, while plain XGBoost reached 0.953.
* **Towards Explainable NIDS using LLMs (August 2024):** GPT-4 and Llama 3 struggled to detect attacks in NetFlow data but were useful for *explaining* alerts.
* **HIDBench (May 2026):** on host-based logs, LLMs reached decent precision on clean data but often fell below 0.5 MCC on noisy, realistic logs.
* **Takeaway:** use classic ML or signatures to *detect*, and an LLM to *explain, summarize and triage*. More on that in [04-The agentic SOC](04-The%20agentic%20SOC.md).

**The Open-Source Engines Kept Evolving (Without the AI Hype)**

The tools recommended in [0201-Network Fundamentals](0201-Network%20Fundamentals.md) are still the backbone of network detection, and none of their major releases in this period was about AI:

* **Suricata 8.0 (July 8, 2025):** an experimental firewall mode (default-drop, with its own rule dialect), the LibHTP HTTP parser rewritten in Rust, new parsers (DoH, LDAP, mDNS, POP3, ARP, WebSocket), sandboxed Lua 5.4 and transactional rules. Security point releases continued through 8.0.8 (October 2026), and 9.0 is in development.
* **Zeek 8.0 LTS (August 2025) and Zeek 9.0 LTS (September 2026):** ZeroMQ became the default cluster backend (Broker is deprecated), and 9.0 added an event for TLS Encrypted Client Hello, a ZIP analyzer and a UDP packet source for encapsulated cloud traffic.
* **Snort:** in January 2026, Cisco Talos ended rule updates for older Snort versions. If you are learning Snort, use a current Snort 3 release.
* **Linux host monitoring:** Falco 0.44.0 (May 2026) removed its legacy eBPF probe and now relies on modern eBPF or the kernel module.

**Host-Based IDS After the CrowdStrike Outage**

On July 19, 2024, a faulty CrowdStrike Falcon content update crashed about 8.5 million Windows machines. The lesson: security agents running inside the kernel are a single point of failure. Microsoft responded with the **Windows Resiliency Initiative** (November 2024), promising to let antivirus/EDR products run outside kernel mode, and a stricter vendor program (MVI 3.0) requiring phased rollouts. As of October 2026, independent analysis by Elastic of Insider builds shows the new Windows Endpoint Security Platform is still in preview, still relies on a Microsoft-built kernel driver, and appears to only *notify* user-mode security tools (not yet let them block). The move out of the kernel is real but unfinished.

**Want to Dive Deeper?**

* [SoK: Decoding the Enigma of Encrypted Network Traffic Classifiers](https://arxiv.org/abs/2503.20093) (S&P 2025)
* [The Sweet Danger of Sugar](https://arxiv.org/abs/2507.16438) (SIGCOMM 2025)
* [netFound: Principled Design for Network Foundation Models](https://arxiv.org/abs/2310.17025) (v5, May 2026)
* [Bad Design Smells in Benchmark NIDS Datasets](https://detlearsom.github.io/pubs/BadSmells-EuroSP24-finaldraft.pdf) (EuroS&P 2024)
* [Network Intrusion Datasets: A Survey, Limitations, and Recommendations](https://arxiv.org/abs/2502.06688)
* [NetFlow datasets v3 (University of Queensland)](https://staff.itee.uq.edu.au/marius/NIDS_datasets/)
* [From Flows to Words: Can LLMs Detect Network Intrusions?](https://arxiv.org/abs/2510.17883) · [Towards Explainable NIDS using LLMs](https://arxiv.org/abs/2408.04342) · [HIDBench](https://arxiv.org/abs/2605.21773)
* [Suricata 8.0.0 release](https://forum.suricata.io/t/suricata-8-0-0-released/5854) · [Introducing Zeek 9](https://zeek.org/2026/09/introducing-zeek-9/) · [Falco 0.44.0](https://falco.org/blog/falco-0-44-0/)
* [The Windows Resiliency Initiative (June 2025 update)](https://blogs.windows.com/windowsexperience/2025/06/26/the-windows-resiliency-initiative-building-resilience-for-a-future-ready-enterprise/) · [Elastic's analysis of WESP](https://www.elastic.co/security-labs/blog/windows-endpoint-security-platform-wesp)

The watchdog did get smarter in these two years, just not in the way the 2024 headlines promised. The real progress was learning how to *test* AI detectors honestly.
