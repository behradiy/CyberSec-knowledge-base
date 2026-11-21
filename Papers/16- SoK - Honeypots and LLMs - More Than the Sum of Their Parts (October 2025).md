## SoK: Honeypots & LLMs, More Than the Sum of Their Parts? (October 2025)

[This paper](https://arxiv.org/abs/2510.25939), by Robert A. Bridges, Thomas R. Mitchell, Mauricio Muñoz and Ted Henriksson (AI Sweden and Volvo Group, Gothenburg), was posted on arXiv in October 2025 and revised in April 2026 (v4). It is a 19-page "Systematization of Knowledge" with 112 references and no experiments of its own: the first attempt to map the whole field of LLM-powered honeypots, which is exactly where this repository's [thesis topic](../Honeypots/) ended up.

**The Challenge: A Fast-Growing Field Without a Map**

* LLMs promised to solve the honeypot's oldest trade-off: a realistic system is risky and expensive, a safe one is easy to spot. Yet the authors find that evaluations show "only incremental progress in real-world deployments".
* Early papers were often written in parallel and "did not cite or build directly upon each other", and no benchmark was widely adopted.
* There was no shared picture of the architectures, the core problems or how to evaluate these systems. The authors found no earlier SoK on the topic.

**The Approach: Three Surveys and a Synthesis**

* **How honeypots get detected:** four "detection vectors" (contents and network posture, outputs and behavior, functional limitations, and combinations of features), each with where LLMs help and where they fail.
* **LLM-powered honeypots:** a Google Scholar search through October 2025, summarized in a table of 16 works from 2023–2025, plus the open-source tools Beelzebub, Galah and T-Pot.
* **LLMs for log analysis:** four phases, from reducing raw logs to classifying sessions, mapping them to MITRE ATT&CK, and agents that analyze on their own.
* **New frameworks:**
  * A reference architecture assembled from recent papers: a server facing the attacker and an LLM at the core, plus optional parts such as a filter that decides when the LLM is needed, a deterministic responder for simple or cached answers, a state manager, a history curator and a prompt builder.
  * An "evaluation tetrad": believability, fidelity, the attacker's cost, and the defender's cost.
  * Three attacker tiers: scripted bots (Tier 1), LLM agents (Tier 2) and skilled humans (Tier 3).
  * A research roadmap.

**Key Findings**

* **Early LLM shells were not convincing enough:** in one study, raters judged more than 1,400 request–response pairs, and "only 52% of the generated responses were convincing".
* **The "data desert":** most real traffic is bots that never test realism. In one three-week deployment, sessions averaged 2.96 commands on the classic Cowrie honeypot against 5.83 on the best LLM honeypot, and the SoK reports that 99.2% of all activity was simple scanning scripts. In two large datasets, only 0.58% and 0.048% of connections became real post-login sessions.
* **Human studies look promising but are small:** nearly 80% of 18 participants found one LLM honeypot (SYNAPSE) more realistic than its static version. The largest study, VelLMes, had 89 participants.
* **Fingerprinting is old, cheap and automatable:** one team used 20 signatures to find more than 19,000 honeypots on the internet, another found more than 21,000, and "these red flags have been reported for almost 20 years". LLM-powered attackers can now run such checks automatically.
* **LLMs are already useful on the analysis side:** one log-analysis model reached over 90% accuracy, and ATT&CK mapping reached 75% precision and 68% recall.
* **Overall verdict:** the "immediate impact has been modest". The authors argue that the right target is not skilled humans but automated, LLM-powered attackers, and that the field is heading toward an autonomous honeypot that loops through interaction, analysis and reconfiguration.

**Conclusion**

LLMs did not make the perfect honeypot appear overnight. They made honeypots more flexible and much better at analyzing what attackers do, but the evidence for real-world deception is still thin, mostly because so little real traffic is interactive enough to test it. The SoK's most useful contribution is a vocabulary (the architecture, the tetrad, the attacker tiers) that later work such as [Honeyval](05-%20Honeyval%20-%20A%20Comprehensive%20Evaluation%20Framework%20for%20LLM-powered%20HTTP%20Honeypots%20%28May%202026%29.md) builds on. For the tools themselves, see [04-LLM-powered honeypots](../Honeypots/04-LLM-powered%20honeypots.md).

**As a Reviewer, My Focus**

* **The selection method is thin for an SoK:** the survey rests on a single Scholar query with no hit counts and no inclusion or exclusion criteria. The text says its table covers "every work in the area", yet it omits two honeypots (MySQL-Pot and DecoyPot) that the paper itself describes later. The April 2026 revision still stops at October 2025, and it summarizes HoneyGPT from its first version (a four-week field study) although the February 2025 revision reports a three-month one.
* **The frameworks are proposed, not applied:** the tetrad is meant to "enable rigorous comparative analysis", but no system is ever scored on it. The claim that LLM agents (Tier 2) are the primary target is explicitly a "conjecture", and it sits uneasily with the paper's own warning that LLM attackers can fingerprint honeypots automatically. Of special interest for this repo: the roadmap proposes information gain as "a formal objective function for optimization algorithms (e.g., Reinforcement Learning)", which is exactly the reward-design question raised in [Papers/03](03-%20Reinforcement%20Learning-assisted%20Threshold%20Optimization%20for%20Dynamic%20Honeypot%20Adaptation%20to%20Enhance%20IoBT%20Networks%20Security%20%28December%202021%29.md), but it stops short of naming an algorithm or a reward formula.
* **Some details don't line up:** the architecture figure marks the filter as optional, while its caption calls it "a critical component for security" that prevents prompt injection, and a later section says injection hardening still needs research. A table calls one study's raters "5 Experts" where the text says graduate students. The 99.2% scanning figure also deserves a check: the freely available version of the cited paper reports about 9.8% effective sessions, which means roughly 90% scripted traffic, and no significance test for the 2.96 vs. 5.83 gap that the SoK calls "statistically significant". (The final published version may differ.)

**Further Exploration**

* [HoneyGPT](https://arxiv.org/abs/2406.01882), one of the field deployments the SoK relies on
* [LogPrécis](https://arxiv.org/abs/2307.08309), LLMs for analyzing honeypot shell logs
* [Honeyval](05-%20Honeyval%20-%20A%20Comprehensive%20Evaluation%20Framework%20for%20LLM-powered%20HTTP%20Honeypots%20%28May%202026%29.md), the benchmark that builds on this SoK
