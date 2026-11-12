## Honeypots vs. AI Hackers: Setting Traps for Machines (2024–2026)

*Covers July 2024 – November 2026.*

Here's a twist nobody expected when this folder started: some of the bees are now robots. Attackers began running LLM-based hacking agents, and defenders noticed that these agents have very predictable weaknesses. They believe what they read. They follow instructions hidden inside data (prompt injection). They have short memories. And they never get bored. All of that makes them almost perfect honeypot victims.

**Are AI Agents Really Attacking? Palisade's Honeypot Says: A Little**

In October 2024, Palisade Research launched the **LLM Agent Honeypot**: a modified Cowrie SSH honeypot with prompt injections hidden in its banner, command outputs and files. A visitor counts as a "confirmed" AI agent only if it obeys an injection *and* answers faster than a human could (about 1.5 seconds).

* **Early 2025:** about 8.1 million connection attempts, 8 *potential* agents.
* **Mid-2026 (old dashboard):** about 24.1 million interactions, 14 potential and 3 confirmed agents.
* **Takeaway:** fully autonomous AI attackers exist in the wild, but they are still rare compared with ordinary bots. The method is a heuristic, so the counts should be read as indicators, not exact numbers.

Other honeypots placed on *AI infrastructure* caught more:

* **OllamaDrama (September 2026):** a fake Ollama API (the popular local-LLM server) ran for 84 days and recorded 290,887 interactions from 2,793 IP addresses. People are actively hunting for exposed AI servers.
* **Zenity (June 2026):** about 10,000 attempts against decoy AI infrastructure. In one case an attacker ran an open-source AI pentest agent from hijacked AI infrastructure against a real e-commerce site, for more than 2,000 unattended steps.
* **Symantec (October 2026):** a decoy AI service captured a complete attacker agent configuration ("Skynet mode") with 74 tools and settings for up to 100,000 unattended steps.
* **OALABS (published June 2026):** researchers turned a compromised host into a honeypot and recovered more than 1,000 sessions in which a low-skill attacker used a stolen AI coding-agent installation to breach at least 14 companies. The attacker got past refusals by claiming it was "authorized red-team" work.

**Prompt Injection as a Defender's Weapon**

Prompt injection is usually an attack *on* AI. Researchers flipped it into a trap *for* AI:

* **Mantis (October 2024):** Decoy services (for example an anonymous FTP server) return prompt injections that humans can't see, hidden with terminal escape codes or HTML comments. The injections derail the attacking agent, or even trick it into opening a reverse shell back to the defender ("hack-back"). The authors report over 95% effectiveness in their own tests. Hack-back raises serious legal questions, so treat this as research.
* **Cloak, Honey, Trap (USENIX Security, August 2025):** 6 strategies and 15 techniques for fighting LLM pentest agents: honeytokens built from Unicode tricks that an LLM notices but a human doesn't, infinite "loop traps", search-space explosions, and more. With the defenses on, PentestGPT running on GPT-4o got initial access on 0 of 33 runs. Full review in [Papers/04](../Papers/04-%20Cloak%2C%20Honey%2C%20Trap%20-%20Proactive%20Defenses%20Against%20LLM%20Agents%20%28August%202025%29.md).
* **AgentSnare (July 2026):** A small fine-tuned model (Qwen3-4B) grows a consistent fake operating system around the attacking agent. It absorbed about half of the attacker's tool calls, 89.5% of the attacker's "mission complete" reports were based on fake evidence, and no real target was exploited across 60 attacker–vulnerability pairs.
* **A Gingerbread House for LLM Agents (November 2026):** "Source-planned" shadow honeypots lure the agent into an isolated copy of the target. Real compromises by a frontier-model agent dropped from 26 of 40 runs to 5 of 40.

**Canary Tokens for the Agent Era**

The Model Context Protocol (MCP) is the standard way AI agents connect to tools (databases, email, file systems). It created a new kind of bait: **fake tools that a legitimate agent should never call**. If one gets called, something has hijacked your agent.

* **Beelzebub MCP honeypot (July 2025):** exposes decoy tools such as a fake "user account manager". A call signals a prompt-injection compromise, and the captured prompts help tune guardrails.
* **Zeltser's decoy MCP server (May 2026):** a small Cloudflare Worker exposing fake "vault" and database tools. Practical gotcha: your *own* agent will trip it unless you exclude it.
* **Thinkst MCP Canarytoken (June 2026):** generates a fake MCP config file (`mcp.json`) for popular coding agents. It alerts when something connects or calls a tool.
* **Thinkst "Agent Provocateur" (September 2026):** a fake AI-backed service that politely asks visiting agents for their plans, username, MAC address and SSH fingerprint. Agents happily answer. Thinkst reports it triggered every time in internal tests (vendor claim, no per-model data).
* **Cloudflare AI Labyrinth (March 2025):** crawlers that ignore "do not crawl" rules get led into an endless maze of AI-generated decoy pages, and following those links fingerprints the bot. It is opt-in, available even on the Free plan, and Cloudflare sees over 50 billion AI-crawler requests per day.

**The Arms Race: Agents That Smell the Honey**

Attackers are adapting. The Rouxii agent (September 2026) was trained to be deception-aware and identified honeypots 97% of the time, and it was even able to knock a classic ICS honeypot (Conpot) offline. The lesson matches the one from [04-LLM-powered honeypots](04-LLM-powered%20honeypots.md): consistency and speed now matter more than realism of any single response.

**Where My Thesis Line Goes Next: Adaptive Deception**

The reinforcement-learning approach reviewed in [Papers/03](../Papers/03-%20Reinforcement%20Learning-assisted%20Threshold%20Optimization%20for%20Dynamic%20Honeypot%20Adaptation%20to%20Enhance%20IoBT%20Networks%20Security%20%28December%202021%29.md) is alive and well:

* **HT-DRL (March 2026):** combines hypergame theory with deep reinforcement learning to steer "honey drones" that draw denial-of-service attacks away from real drone missions, improving mission performance by up to 2×. It is the closest modern relative of the IoBT honeypot paper.
* **Towards Agentic Honeynet Configuration (AgenNet@NOMS 2026):** an LLM agent reads IDS alerts and reconfigures which honeypots are exposed. Results are preliminary, but it connects the [IDS](../IDS/) and Honeypots folders of this repo nicely.

**The Business of Deception**

* **January 2026:** Thinkst (Canary) acquired the UK deception company DeceptIQ.
* **March 2026:** Tracebit raised a $20M Series A for cloud canaries.

**Want to Dive Deeper?**

* [LLM Agent Honeypot: Monitoring AI Hacking Agents in the Wild](https://arxiv.org/abs/2410.13919) and the [live dashboard](https://ai-honeypot.palisaderesearch.org/)
* [Hacking Back the AI-Hacker (Mantis)](https://arxiv.org/abs/2410.20911)
* [Cloak, Honey, Trap](https://www.usenix.org/conference/usenixsecurity25/presentation/ayzenshteyn)
* [AgentSnare](https://arxiv.org/abs/2607.26998) · [Gingerbread House](https://arxiv.org/abs/2611.07611) · [Rouxii](https://arxiv.org/abs/2609.26555)
* [OllamaDrama](https://arxiv.org/abs/2609.29757) · [Zenity's AI-infrastructure honeypots](https://zenity.io/blog/real-world-ai-infrastructure-attacks-honeypots) · [Symantec on "Skynet mode"](https://www.security.com/threat-intelligence/ai-llmjacking-skynet)
* [OALABS: an attacker's AI agent sessions](https://research.openanalysis.net/claude/codex/hacking/ai%20hacking/llm/redteam/policy%20violation/2026/06/16/compromised-claude-hacking.html)
* [Thinkst MCP Canarytoken](https://blog.thinkst.com/2026/06/one-for-all-the-models-out-there.html) · [Agent Provocateur](https://blog.thinkst.com/2026/09/getting-agents-to-tell-on-themselves.html) · [Zeltser's decoy MCP server](https://zeltser.com/decoy-mcp-server-honeypot)
* [Cloudflare AI Labyrinth](https://blog.cloudflare.com/ai-labyrinth/)
* [HT-DRL](https://arxiv.org/abs/2603.20981) · [Towards Agentic Honeynet Configuration](https://arxiv.org/abs/2603.14122)

For twenty years honeypots were built to fool humans. The next generation is built to fool machines, and it turns out machines are much easier to fool, at least for now.
