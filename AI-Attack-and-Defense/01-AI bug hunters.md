## AI Bug Hunters: Machines That Find (and Fix) Vulnerabilities (2024–2026)

*Covers July 2024 – November 2026.*

In 2021, the CSET report reviewed in [Papers/02](../Papers/02-%20Machine-Learning-and-Cybersecurity.md) listed "advanced vulnerability discovery" as something that would need real breakthroughs. Five years later, those breakthroughs arrived faster than almost anyone predicted. AI systems now find serious bugs in some of the most audited software on Earth: SQLite, OpenSSL, the Linux kernel, Firefox. This file tells that story in three acts, then looks at the problems it created.

**Act 1: First Sparks (Late 2024)**

* **Big Sleep finds a real bug (November 1, 2024):** Google Project Zero and DeepMind's agent found an exploitable memory-safety bug in SQLite. It was fixed the same day, before it ever reached a release, so it has no CVE. Google itself called the work "highly experimental". Still, it was the first public case of an AI agent finding a new bug of this kind in widely used software.
* **AI-written fuzz targets (November 2024):** Google's OSS-Fuzz used LLMs to write *fuzzing harnesses*, the glue code that lets a fuzzer test a function. This found 26 new vulnerabilities, including OpenSSL CVE-2024-9143 (moderate severity) in code Google estimates had been there for about 20 years. This "LLM writes the harness, fuzzer proves the bug" pattern produces very few false alarms.

**Act 2: The Year It Got Real (2025)**

* **One researcher, one model, one kernel zero-day (May 2025):** Sean Heelan pointed OpenAI's o3 at hand-picked Linux kernel SMB code and it found a remote use-after-free (CVE-2025-37899). Context matters: on a benchmark bug it succeeded in only 8 of 100 runs, with roughly 1 true finding per 50 false ones.
* **XBOW tops HackerOne (June 2025):** an autonomous pentesting system reached #1 on HackerOne's US 90-day leaderboard after about 1,060 submissions. Humans reviewed every report before it was sent, as HackerOne requires.
* **Big Sleep and CVE-2025-6965 (July 2025):** Google said the agent found a SQLite flaw that only threat actors knew about, and called it the first time an AI agent had foiled an exploitation attempt in the wild. That last part is Google's claim, and threat-intelligence leads guided the search.
* **DARPA AI Cyber Challenge final (August 8, 2025, DEF CON 33):** the biggest public test yet. Seven fully autonomous "cyber reasoning systems" ran on real open-source projects (over 54 million lines of code) seeded with 63 synthetic bugs. They found 54 and patched 43, and also found 18 real, previously unknown vulnerabilities. A patch took about 45 minutes on average, and each task cost about $152. **Team Atlanta** (Georgia Tech, Samsung Research, KAIST, POSTECH) won $4M, **Trail of Bits** $3M and **Theori** $1.5M, and all finalist systems were open-sourced. Full analysis in [Papers/07](../Papers/07-%20SoK%20-%20DARPA%27s%20AI%20Cyber%20Challenge%20%28AIxCC%29%20%28February%202026%29.md).
* **Google DeepMind CodeMender (October 2025):** an agent that *fixes* rather than finds. In six months it upstreamed 72 security fixes to open-source projects, some as large as 4.5 million lines. It checks its own patches with fuzzing, differential testing and formal methods, and humans review every patch.
* **OpenAI Aardvark (October 2025):** an "agentic security researcher" in private beta, renamed **Codex Security** in March 2026.

**Act 3: Frontier Models Go Big (2026)**

* **February 2026:** Anthropic reported that its Opus 4.6 model, with standard tools and no special setup, found over 500 validated high-severity memory-corruption bugs in open-source projects. The same day, OpenAI's GPT-5.3-Codex became its first model treated as "High" capability for cybersecurity, released alongside an identity-verified "Trusted Access for Cyber" program.
* **Project Glasswing (April 7, 2026):** Anthropic announced its Mythos Preview model and declined to release it publicly. Access went to 12 launch partners and about 40 other organizations, for defensive use only, backed by $100M in usage credits. Anthropic claimed zero-days in every major operating system and browser, including a 27-year-old OpenBSD bug and a FreeBSD remote-code-execution exploit written autonomously.
  * **Independent checks:** the UK AI Security Institute found it was the first model to complete its 32-step simulated corporate-network attack (in 3 of 10 attempts). Mozilla fixed 271 Firefox bugs found with it in Firefox 150.
  * **The bottleneck (May 2026 update):** of about 530 bugs disclosed to open-source maintainers, only 75 had been patched, and some maintainers asked Anthropic to slow down.
* **First AI-built exploit seen in the wild (May 2026):** Google's threat intelligence group reported, with high confidence, that criminals used an AI model to find and weaponize a new flaw that bypassed two-factor login in an open-source admin tool. It was caught before mass exploitation (more in [02-How attackers use AI](02-How%20attackers%20use%20AI.md)).
* **October 2026:** an XBOW researcher earned $250,000 from Google's Chrome bug bounty for a full exploit chain built with AI agents over six days. Anthropic folded Glasswing into a tiered "Cyber Verification Program".
* **Industrial scale (November 4–5, 2026):** OpenAI reported that an unreleased model generated 1,169 working Linux kernel privilege-escalation exploits covering 948 distinct bugs, for about $1.2 million in compute. A day later it launched **GPT-6 Cyber**, rated "Critical" for cybersecurity under its own Preparedness Framework, and available only to vetted customers of its "Daybreak" program. All of these numbers come from OpenAI itself.

**The Scoreboard: Benchmarks Over Time**

Labs use different settings, so treat these as trends, not exact comparisons.

| Benchmark | What it measures | How scores moved |
|---|---|---|
| [Cybench](https://arxiv.org/abs/2408.08926) | 40 capture-the-flag tasks | ~36% (early 2025) to ~77% (late 2025); now close to saturated |
| [CVE-Bench](https://arxiv.org/abs/2503.17332) | Exploiting real web-app CVEs | 13% at release (March 2025) to 90% (February 2026) |
| [CyberGym](https://arxiv.org/abs/2506.02548) | Reproducing 1,507 real vulnerabilities | ~20% at release (June 2025) to 83.1% (April 2026) |
| [BountyBench](https://arxiv.org/abs/2505.15216) | Detect, exploit and patch real bug-bounty bugs | Patching (90%) far easier than detecting (12.5%) |
| [ExploitGym](https://arxiv.org/abs/2605.11086) | Turning 898 known crashes into working exploits | Best model 157 of 898; scores collapse when exploit mitigations are on |

**The Catch: Problems Nobody Has Solved Yet**

* **AI slop:** curl ended its bug bounty on January 31, 2026, after the share of valid reports fell from over 15% to under 5% in 2025, buried under AI-generated junk.
* **Patching can't keep up:** finding bugs is now cheap; fixing, reviewing and releasing them still depends on a small number of human maintainers.
* **Vendor claims vs. independent evidence:** independent reviews (Epoch AI, AISLE, curl's own test) support a real jump in *exploit building*, but find the evidence for *bug finding* more anecdotal. In one curl scan, 5 "confirmed" issues became 1 low-severity CVE and 3 false positives.
* **Patches that look right but aren't:** in the AIxCC analysis, 37.7–45.6% of semantically wrong LLM patches still passed automated validation.
* **Dual use:** every capability above also helps attackers. See [02-How attackers use AI](02-How%20attackers%20use%20AI.md).

**What This Means for Defenders**

* **Patch faster:** the time from disclosure to exploitation is shrinking. In one case an AI-discovered bug was exploited 4 days after disclosure.
* **Combine fuzzing and LLMs:** the strongest results pair a fuzzer (to prove a bug is real) with an LLM (to aim it).
* **Know your dependencies:** a flood of new CVEs is coming. An accurate software inventory (SBOM) decides how fast you can react.
* **Validate AI-generated patches:** with real tests and human review before you merge them.

**Want to Dive Deeper?**

* [From Naptime to Big Sleep](https://projectzero.google/2024/10/from-naptime-to-big-sleep.html) · [Leveling up fuzzing with AI](https://security.googleblog.com/2024/11/leveling-up-fuzzing-finding-more.html)
* [How I used o3 to find CVE-2025-37899](https://sean.heelan.io/2025/05/22/how-i-used-o3-to-find-cve-2025-37899-a-remote-zeroday-vulnerability-in-the-linux-kernels-smb-implementation/)
* [DARPA AIxCC results](https://darpa.mil/news/2025/aixcc-results) · [AIxCC archive with all finalist systems](https://archive.aicyberchallenge.com/) · [SoK paper](https://arxiv.org/abs/2602.07666)
* [Introducing CodeMender](https://deepmind.google/discover/blog/introducing-codemender-an-ai-agent-for-code-security/)
* [Anthropic: Mythos Preview](https://www.anthropic.com/research/mythos-preview) · [Glasswing initial update](https://www.anthropic.com/news/glasswing-initial-update) · [UK AISI evaluation](https://aisi.gov.uk/blog/our-evaluation-of-claude-mythos-previews-cyber-capabilities) · [Mozilla: hardening Firefox](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/)
* [OpenAI GPT-5.3-Codex cybersecurity system card](https://deploymentsafety.openai.com/gpt-5-3-codex/cybersecurity)
* [SecurityWeek: AI agents got root on Linux via 948 kernel flaws](https://www.securityweek.com/ai-agents-got-root-on-linux-via-948-kernel-flaws-in-openai-research/) · [The Hacker News: GPT-6 Cyber](https://thehackernews.com/2026/11/openai-launches-gpt-6-cyber-after-ai.html)
* [The end of the curl bug bounty](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/) · [Epoch AI: Are Mythos's cyber capabilities overhyped?](https://epochai.substack.com/p/are-mythos-cyber-capabilities-overhyped)

The bug hunters are no longer only human. Whether that makes software safer depends on whether we can fix bugs as fast as machines can now find them.
