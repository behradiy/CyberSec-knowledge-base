## SoK: DARPA's AI Cyber Challenge (AIxCC): Competition Design, Architectures, and Lessons Learned (February 2026)

[This paper](https://arxiv.org/abs/2602.07666), by Cen Zhang, Younggi Park, Fabian Fleischer, Yu-Fu Fu, Taesoo Kim and 16 co-authors, is a "Systematization of Knowledge" (SoK) accepted at USENIX Security 2026. It is the first systematic analysis of DARPA's AI Cyber Challenge (2023–2025), the largest competition so far for fully autonomous systems that find *and fix* vulnerabilities in real open-source software.

**The Challenge: Can Machines Secure Open Source on Their Own?**

* Open-source software underpins critical infrastructure, but there are far too few people to audit it.
* DARPA asked teams to build **Cyber Reasoning Systems (CRSs)**: fully autonomous programs that combine classic techniques (fuzzing, static analysis) with large language models to discover bugs, prove them with a crashing input, and submit patches, with no human in the loop.

**The Competition in Numbers**

* **Field:** 42 entrants, 7 finalists, a final round running fully autonomously for 143 hours.
* **Targets:** 48 scored challenges from 24 OSS-Fuzz projects, seeded with 63 synthetic vulnerabilities (40 in C, 23 in Java).
* **Budget:** each team had $85,000 of Azure compute and $50,000 of LLM credits.
* **Result:** Team Atlanta (Georgia Tech, Samsung Research, KAIST, POSTECH) won with 392.8 points, ahead of Trail of Bits (219.4) and Theori (210.7). An accuracy penalty of −16.3 points cost Theori second place.

**Key Findings**

* **Fuzzing is still the backbone:** plain parallel fuzzing alone would have proved 34 of the 63 vulnerabilities, but only 4 of the 23 in Java. The finalists solved 7–16 more than fuzzing alone, which the authors credit mostly to LLM components.
* **Patch validation is the weak spot:** between 37.7% and 45.6% of LLM patches that were semantically *wrong* still passed the automated validation.
* **Real impact:** across the final, the systems found 25 distinct real (not synthetic) bugs in 10 projects, and 12 of them were patched.
* **Lessons for future competitions:** the paper analyzes which design decisions (scoring, budgets, infrastructure) shaped team strategies, and what it would take to deploy such systems in practice.

**Conclusion**

AIxCC showed that autonomous AI systems can find and fix real vulnerabilities at scale, but the gains came from *combining* LLMs with decades-old techniques like fuzzing, not from replacing them. The paper is the best single source for understanding what actually drove performance, beyond the scoreboard.

**As a Reviewer, My Focus**

* **Independence:** the lead authors come from the winning team and from the organizers. The analysis is detailed and honest about weaknesses, but an independent replication would carry more weight.
* **Missing ablations:** the split between "what the LLM did" and "what the fuzzer did" is inferred from traces, not tested with controlled experiments (for example, running each system with its LLM components switched off). That is the most important question for the field, and it deserves a direct answer.
* **Will it work outside the arena?** The bugs were planted, the models were frozen in 2025, and the paper's count of real bugs (25) differs from DARPA's official figure (18 found, 11 patched). A companion paper (OSS-CRS, [arXiv:2603.08566](https://arxiv.org/abs/2603.08566)) reports that the released systems are hard to run outside the competition infrastructure. A follow-up on real-world deployment would be very valuable.

**Further Exploration**

* [DARPA's official AIxCC results](https://darpa.mil/news/2025/aixcc-results)
* [AIxCC archive (all finalist systems)](https://archive.aicyberchallenge.com/)
* [01-AI bug hunters](../AI-Attack-and-Defense/01-AI%20bug%20hunters.md) in this repo
