## CyberGym: Evaluating AI Agents' Real-World Cybersecurity Capabilities at Scale (June 2025)

[This paper](https://arxiv.org/abs/2506.02548), by Zhun Wang, Tianneng Shi, Jingxuan He, Matthew Cai, Jialin Zhang and Dawn Song (UC Berkeley), was first posted on arXiv in June 2025 and published at ICLR 2026. This review covers the third version (March 2026). CyberGym became one of the standard yardsticks that AI labs now quote when they announce a new model's cyber capabilities.

**The Challenge: Benchmarks Were Too Small and Too Artificial**

* Earlier security benchmarks had at most 200 tasks: Cybench, CVE-Bench and BountyBench have 40 each.
* Capture-the-flag puzzles are idealized and miss the complexity of real codebases.
* Benchmarks measure only historical tasks and say nothing about whether an agent could affect *current* software.
* Reproducing a vulnerability from a text report is genuinely hard: the authors estimate about 5 hours for a human, and fuzzers take a median of 324 days to reveal these bugs.

**The Benchmark: 1,507 Real Vulnerabilities**

* **Scale:** 1,507 memory-safety bugs from 188 open-source C/C++ projects, disclosed between 2017 and April 2025. Most (1,368) come from the ARVO dataset of OSS-Fuzz bugs; 139 newer ones were collected by the authors.
* **Real code:** the median project has 1,117 files and about 387,000 lines of code.
* **The task:** given the source code and a short description of the bug, write a proof-of-concept input (PoC) that triggers it. The agent can test its PoC against the vulnerable program as often as it likes.
* **What counts as success:** the PoC must crash the program before the fix and *not* crash it after the fix, as detected by sanitizers.
* **Difficulty levels:** from source code only (level 0) up to also receiving the crash stack trace (level 2) or the patch itself (level 3). Level 1 (code plus description) is the main setting.
* **Descriptions:** an LLM rewrites each fix's commit message into a bug description, and a human audit of 300 samples found 96% precision.
* **Tested:** 4 agent frameworks and 11 models, with a budget of 100 steps and about $2 per task. The full study used over $40,000 in API credits plus 1,000 GPU hours.

**Key Findings**

* **The best agent solved 17.9% of the full benchmark:** that was Anthropic's Sonnet 4 with the OpenHands framework. Other models followed: GPT-4.1 9.4%, GPT-5 7.8% (minimal reasoning), and most open models below 4%. All models together solved 27.2%.
* **Thinking helps:** on a 300-task subset, GPT-5 with extended reasoning reached 22.0%, the paper's best number. The original June 2025 version's best was 11.9%.
* **The framework matters less than the model:** with GPT-4.1, the four frameworks scored between 7.2% and 9.4%, and 18.4% together.
* **Hints help a lot:** success rose from 3.5% with code only to 9.4% with a description, 13.1% with a stack trace and 17.1% with the patch.
* **Long inputs are hard:** success was 55.3% when the correct PoC was under 10 bytes, but "around 10%" above 100 bytes, which covers about two-thirds of the benchmark.
* **Little sign of memorization:** success rates before and after each model's training cutoff were not significantly different (all p-values above 0.1).
* **Real impact:** the agents found **34 previously unknown vulnerabilities** (zero-days) and 18 incomplete patches. 4 CVEs were assigned and 10 bugs patched. Nine of the zero-days came from PoCs that still crashed the latest software; 25 more came from an open-ended run across 431 projects.

**Conclusion**

CyberGym showed that AI agents could reproduce a meaningful share of real vulnerabilities in large codebases, and, almost as a side effect, find new ones. Its scale made it the benchmark labs reached for: by April 2026, a lab reported 83.1% for its own unreleased model in its own harness, so the benchmark is approaching saturation. For how these numbers fit the bigger story, see [01-AI bug hunters](../AI-Attack-and-Defense/01-AI%20bug%20hunters.md).

**As a Reviewer, My Focus**

* **No variance, and the best number comes from a subset:** there are no repeated runs, seeds or confidence intervals for any result. The headline "~20%" combines 17.9% on the full set with 22.0% on a 300-task subset, and that subset is itself noisy: one model scored 14.3% on it but 11.9% on the full set, a gap larger than several of the reported "thinking" gains. The claim that CyberGym is a reliable proxy for real-world skill rests on comparing just two models in the zero-day campaign.
* **A crash is not an exploit:** success only requires a crash before the patch and none after it. The crash is never matched to the original bug's location, so an agent can succeed by triggering a different bug that the patch happens to fix. 24 of the 34 zero-days are null-pointer dereferences or stack exhaustion, mostly denial-of-service bugs. The authors list exploitation as future work, which their follow-up [ExploitGym](12-%20ExploitGym%20-%20Can%20AI%20Agents%20Turn%20Security%20Vulnerabilities%20into%20Real%20Attacks%20%28May%202026%29.md) took on.
* **Level 1 is closer to a one-day than it sounds:** the descriptions are rewritten fix commits, deliberately filtered to keep commits that point to the bug's location. The contamination test covers only four models (not the best one), with 88–142 post-cutoff samples, so it has little statistical power. Costs are reported only in total. The latest version also contradicts itself in places: its introduction still says "17 inadequate historical patches and 10" zero-days, while the abstract and results say 18 and 9.

**Further Exploration**

* [CyberGym code](https://github.com/sunblaze-ucb/cybergym) · [Leaderboard](https://www.cybergym.io/cybergym/)
* [Cybench](https://arxiv.org/abs/2408.08926), the earlier CTF-style benchmark
* [SoK: DARPA's AI Cyber Challenge](07-%20SoK%20-%20DARPA%27s%20AI%20Cyber%20Challenge%20%28AIxCC%29%20%28February%202026%29.md), where fuzzers and LLMs worked together on similar bugs
