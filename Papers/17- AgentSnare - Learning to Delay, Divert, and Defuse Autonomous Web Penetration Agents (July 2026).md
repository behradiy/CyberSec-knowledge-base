## AgentSnare: Learning to Delay, Divert, and Defuse Autonomous Web Penetration Agents (July 2026)

[This paper](https://arxiv.org/abs/2607.26998), by Ruoyu Wang, Heng Zhao, Renjie Wu, Tianhang Zheng, Mengnan Zhao, Wanyu Lin and Kui Ren, comes from Zhejiang University, the University of Hong Kong, Hainan University, Anhui University and Hong Kong Polytechnic University. It was first posted on arXiv in July 2026, and this review covers the fifth version (November 2026), a 12-page preprint with a larger evaluation than the original. It asks a question at the heart of this repository: can a decoy that *learns* how AI attackers behave keep them busy and away from the real target?

**The Challenge: AI Attackers Ignore Static Traps**

* AI pentest agents work in a loop: observe, act, observe again. That gives a defender a lever, since whatever the agent observes shapes what it does next.
* Static decoys planted in advance lose their grip over long runs. In the authors' motivating test with six models and six deception tactics, average interference was below 0.2.
* Agents ignore clues that don't fit their current hypothesis, test cheap clues in one or two checks, and prefer paths they have already verified on the real target.
* Running a frontier LLM as the decoy would be too slow and expensive for most web applications.

**The Approach: A Fake OS That Grows on Demand**

* **ShadowOS:** a virtual decoy operating system that sits *next to* the real web application. When the attacker probes something that doesn't exist yet, a small model proposes a believable answer. A deterministic "virtual kernel" checks that answer for consistency and safety, then records it in a ledger of world facts. Repeated probes are answered from the ledger without calling the model, which keeps the story consistent. Actions aimed at the real application never reach ShadowOS.
* **Training without RL:** two LLM agents (DeepSeek-V4-Flash) play attacker and teacher in simulation. At each step the teacher proposes 3 candidate answers, the simulated attacker samples 4 follow-up actions for each, and the candidate that keeps the most follow-ups inside the decoy wins. The resulting dataset (24,807 decisions from 503 trajectories) is used to fine-tune **Qwen3-4B** with LoRA, served from a single 48 GB GPU.
* **Luring attackers in:** a reverse proxy adds breadcrumbs to the real site (debug headers, HTML comments, `robots.txt` entries, decoy files) that point to an unauthenticated SSH entry into the decoy.
* **Evaluation:** 15 critical web vulnerabilities from CVE-Bench, attacked by the PentestGPT agent running four models (Anthropic's Opus 4.6, OpenAI's GPT-5.4, MiniMax M3 and GLM-5.3), three runs each (540 defended runs). The baselines are 13 static deception tactics, the classic Cowrie honeypot and an LLM honeypot (HoneyLLMd).

**Key Findings**

* **Delay:** AgentSnare absorbed **49.9%** of the attackers' tool calls (37.7–59.3% depending on the model), against 15.6% for Cowrie and 28.4% for HoneyLLMd.
* **Divert:** once inside, 57.2% of the attackers' actions stayed in the decoy, against 34.3% for HoneyLLMd.
* **Defuse:** **89.5%** of the attackers' "task complete" submissions (648 of 724) rested on fake evidence from the decoy.
* **No real target was exploited:** without any defense, the attackers exploited 29 of the 60 attacker–vulnerability pairs. Static tactics changed nothing (still 29), Cowrie cut it to 19, HoneyLLMd to 13, and AgentSnare to **0 of 60**. The 13 static tactics left every outcome unchanged.
* **The training matters:** in an ablation with a fifth attacker model, the fine-tuned 4B model allowed 0 of 15 exploits, against 3 for the same model without fine-tuning and 1 for a much larger frontier model plugged into the same ShadowOS.
* **A deception-aware attacker:** against Rouxii, an attacker built to spot honeypots, AgentSnare still allowed 0 exploits (vs. 1 for HoneyLLMd and 3 with no defense), but the share of submissions resting on fake evidence fell from 96.5% to 77.6%.
* **Cost:** about 8 GB of GPU memory and a median latency of 157–161 ms from 1 to 32 parallel sessions. Running the decoy on frontier APIs instead would cost an estimated $0.72–$3.61 per session.

**Conclusion**

A decoy generated on the fly, but kept consistent by deterministic code, held AI pentest agents far longer than static traps or earlier honeypots, and most of their "successes" turned out to be fake. Notably, a 4-billion-parameter model trained on simulated attacker reactions was enough, at the cost of one local GPU. It is a concrete answer to the question [Papers/04](04-%20Cloak%2C%20Honey%2C%20Trap%20-%20Proactive%20Defenses%20Against%20LLM%20Agents%20%28August%202025%29.md) left open: static tricks against AI attackers fade, adaptive ones last longer. See [05-Deception against AI attackers](../Honeypots/05-Deception%20against%20AI%20attackers.md) for the wider picture.

**As a Reviewer, My Focus**

* **"0 of 60" is weaker than it reads:** only 29 of the 60 pairs were exploitable without any defense, and two of the four attackers managed just 3 and 2 of 15. With differences of one CVE visible between runs (HoneyLLMd actually *raised* one attacker from 2 to 3), the exploit counts need confidence intervals, which the paper doesn't give. The no-defense counts are also labeled pass@1 in the earlier versions and pass@3 in the latest one, with identical numbers.
* **A narrow, mostly unwarned setting:** every main attacker runs through one agent framework and enters through the same unauthenticated SSH breadcrumb. Only one deception-aware attacker is tested, and against it the defuse rate drops by almost 19 points. The ablation margins over a frontier model inside the same ShadowOS are as small as 0.2 points, without error bars. For the three questions raised in [Papers/03](03-%20Reinforcement%20Learning-assisted%20Threshold%20Optimization%20for%20Dynamic%20Honeypot%20Adaptation%20to%20Enhance%20IoBT%20Networks%20Security%20%28December%202021%29.md), the answer here is not RL at all: the "algorithm" is supervised fine-tuning, the closest thing to a reward is the retention score, and exploration is simple sampling with a greedy choice at each step.
* **Operational claims are untested:** the breadcrumbs are added to every response, yet there is no analysis of their effect on real users, search-engine crawlers or authorized scanners. There is no limitations or ethics section and no code release. "Delay" counts tool calls within a fixed budget of 200, not wall-clock time, and the "$0.00" API cost leaves out the cost of hosting the GPU.

**Further Exploration**

* [CVE-Bench](https://github.com/uiuc-kang-lab/cve-bench), the vulnerable web apps used as targets
* [Rouxii](https://arxiv.org/abs/2609.26555), the deception-aware attacker used in the adaptive test
* [LLM Agent Honeypot](10-%20LLM%20Agent%20Honeypot%20-%20Monitoring%20AI%20Hacking%20Agents%20in%20the%20Wild%20%28October%202024%29.md), which detects AI agents in the wild rather than steering them
