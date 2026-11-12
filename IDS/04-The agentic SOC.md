## The Agentic SOC: Robot Analysts on the Night Shift (2024–2026)

*Covers July 2024 – November 2026.*

An IDS raises the alarm; somebody still has to answer it. In a Security Operations Center (SOC), that somebody is an analyst drowning in thousands of alerts a day, most of them false. Between 2025 and 2026 every major security vendor shipped "agents": AI assistants that don't just chat, but pull logs, query databases, enrich alerts and write up a verdict while the human sleeps. The watchdog now comes with a robot dog-handler.

**What Does "Agentic SOC" Actually Mean?**

* **Triage:** the agent reads an alert, gathers context (user, host, past alerts, threat intel) and labels it "likely benign" or "escalate".
* **Investigation:** it writes and runs queries across logs (for example KQL in Microsoft Sentinel), following leads from one entity to the next.
* **Detection engineering:** it drafts new detection rules (Sigma, KQL) from threat reports.
* **Response:** it proposes or, with permission, runs containment steps.

**What the Big Vendors Shipped**

* **Microsoft Security Copilot agents:** launched March 2025 with 6 Microsoft agents (including Phishing Triage) and 5 partner agents. From November 2025, Security Copilot is bundled with Microsoft 365 E5, with 12 new Microsoft agents and more than 30 partner agents. Analyst and alert-triage agents for identity and cloud alerts entered preview in March 2026.
* **Google:** Sec-Gemini v1 (April 2025), an experimental security model offered only for research. Google Unified Security went GA in April 2025, and the SecOps Triage and Investigation agent is GA (Google says it has investigated over 5 million alerts as of June 2026). Threat-hunting and detection-engineering agents entered preview in August 2026.
* **CrowdStrike:** Charlotte AI AgentWorks (a no-code agent builder, fall 2025), and multi-agent investigations, generally available from November 2026.
* **Palo Alto Networks:** Cortex AgentiX (October 2025), positioned as the next generation of its XSOAR automation platform.

**Careful:** marketing numbers like "550% faster", "98% lower response time" or "30 minutes to 60 seconds" come from the vendors themselves, without independent validation.

**What the Evidence Says**

* **The first randomized controlled trial (Microsoft, November 2025):** 167 external analysts triaged phishing-email queues with or without an AI agent. With the agent they reached F1 0.93 against 0.53, and found 6.5× more true positives per minute. The twist: about 80% of the gain came from the agent *reordering the queue*, and only about 15% from its verdicts and explanations. The study is vendor-authored, and the agent happened to be 100% accurate on the test emails.
* **A 10-month field study (CSIRO and eSentire, August 2025):** 45 analysts made 3,090 GPT-4 queries. They used it to *understand* data (decode a script, explain a log line), not to make decisions.
* **How triage agents fail (ALERT-BENCH, October 2026):** 1,247 alerts replayed through a live SIEM. Five common LLM triage designs each missed at least 40% of real attack alerts. The typical failure: a log search comes back empty, and the agent concludes "nothing happened". A propose-challenge-judge design (AIDA) reached F1 0.958 with a 3.1% miss rate, by escalating 18.4% of alerts to humans.
* **The SANS 2026 SOC survey:** 79% of SOCs use AI/ML in some form, but only 36% have built it into a defined workflow.

**Benchmarks You Should Know**

| Benchmark | Year | What it tests | Headline result |
|---|---|---|---|
| [ExCyTIn-Bench](https://arxiv.org/abs/2507.14201) (Microsoft, ICML 2026) | 2025 | Investigating 8 multi-stage attacks across 57 log tables | Best models score ~0.6 of 1.0 |
| [CyberSOCEval](https://arxiv.org/abs/2509.20166) (Meta + CrowdStrike) | 2025 | Malware analysis and threat-intel reasoning (multiple choice) | Roughly 23–34% on malware, 43–53% on threat intel |
| [CTI-REALM](https://www.microsoft.com/en-us/security/blog/2026/03/20/cti-realm-a-new-benchmark-for-end-to-end-detection-rule-generation-with-ai-agents/) (Microsoft) | 2026 | Writing Sigma/KQL rules from threat reports, checked against attack telemetry | Top scores ~0.62–0.69; cloud tasks only ~0.28 |
| [AVDA](https://arxiv.org/abs/2603.25930) | 2026 | Auto-generated detection rules | 99.4% matched the right technique, but only 8.9% matched the false-positive exclusions a human would add |
| [ALERT-BENCH](https://arxiv.org/abs/2610.10608) | 2026 | Alert triage in a live SIEM | Baselines miss ≥40% of attacks |

**A gap worth noticing:** these benchmarks cover Sigma and KQL. As of late 2026 there seems to be no comparable public benchmark for LLM-generated **Suricata or YARA** rules. Great project idea for anyone following the [IDS roadmap](02-what%20should%20you%20learn.md).

**Practical Takeaways**

* **Start with phishing triage:** it is the most mature, best-measured use case.
* **Measure before you trust:** track misses (false negatives), not just time saved.
* **"No evidence" is not "no attack":** make sure the agent escalates when a search returns nothing, rather than closing the alert.
* **Keep humans on the decision:** agents are best at gathering context and ordering the queue.
* **Watch the meter:** agent features are billed by usage, and costs can surprise you.

**Want to Dive Deeper?**

* [Randomized Controlled Trials for Phishing Triage Agent](https://arxiv.org/abs/2511.13860)
* [LLMs in the SOC: An Empirical Study of Human-AI Collaboration](https://arxiv.org/abs/2508.18947)
* [From Investigation Failures to Reliable SOC Agents (ALERT-BENCH)](https://arxiv.org/abs/2610.10608)
* [Microsoft Security Copilot agents announcement (March 2025)](https://www.microsoft.com/en-us/security/blog/2025/03/24/microsoft-unveils-microsoft-security-copilot-agents-and-new-protections-for-ai/)
* [Google Security Operations agents (June 2026)](https://cloud.google.com/blog/products/identity-security/detecting-and-containing-powered-threats-with-google-security-operations-agents)
* [Sec-Gemini v1](https://blog.google/security/google-launches-sec-gemini-v1-new/)
* [CrowdStrike: next evolution of the agentic SOC](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-next-evolution-of-the-agentic-soc/)
* [SANS Security Operations Report](https://www.sans.org/research/security-operations-report)

The robot analyst is real, useful and still learning. The best results so far come from pairing it with a human who checks the robot's homework.
