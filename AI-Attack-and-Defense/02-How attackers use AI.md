## The Dark Side: How Attackers Actually Used AI (2024–2026)

*Covers July 2024 – November 2026. Everything here comes from published threat-intelligence reports; claims made only by vendors are marked as such.*

Every security conference since 2023 has warned about "AI-powered hackers". This file separates what was predicted from what was actually observed. The short version: attackers climbed a ladder, one rung at a time, and by late 2026 they were on the upper rungs, though not at the top.

**Rung 1: AI as a Productivity Tool (2024–2025)**

* **Google, January 2025:** state-backed groups from more than 20 countries used Gemini, most of them from China (20+ groups) and Iran (10+). They used it for research, coding help, phishing lures and translation. Their jailbreak attempts were low-effort and failed. Google saw no *new* capabilities.
* **OpenAI, October 2025:** after disrupting more than 40 malicious networks, OpenAI concluded that its models gave attackers small efficiency gains, not new offensive powers.
* **Phishing that works:** a 2024 study with 101 participants (Heiding, Schneier et al.) found fully AI-generated spear-phishing emails got a 54% click rate, the same as emails written by human experts and far above the 12% control. Each email cost about $0.04. The paper is in the [Papers reading list](../Papers/README.md).
* **AI-written malicious code leaves fingerprints:** in September 2025, Microsoft caught a phishing campaign whose hidden code was clearly LLM-generated: overly long variable names, generic comments, over-engineered structure. It was blocked by infrastructure and behavior signals, not by reading the code.
* **Voice clones:** in May 2025 the FBI warned that criminals were using AI-generated voice messages to impersonate senior US officials and take over accounts. A familiar voice is no longer proof of identity.

**Rung 2: Malware That Talks to an LLM While It Runs (2025–2026)**

* **LAMEHUG / PROMPTSTEAL (July 2025):** Ukraine's CERT-UA found malware, attributed with moderate confidence to the Russian group APT28, that sent a task description to an open-source coding model (Qwen2.5-Coder) via Hugging Face's API and ran whatever commands came back. Google calls it the first malware it saw querying an LLM in live operations.
* **Google's tracker (November 2025):** it lists several malware families that call LLMs. PROMPTSTEAL, QUIETVAULT and FRUITSHELL were seen in real operations. QUIETVAULT even uses AI command-line tools already installed on the victim's machine to hunt for secrets, and FRUITSHELL carries prompts designed to fool AI-based malware analysis. PROMPTFLUX and PROMPTLOCK were only experimental.
* **2026 additions:** HONESTCUE (asks Gemini to write its second stage, then compiles it in memory) and PROMPTSPY (Android malware that sends the screen layout to an LLM and gets back where to tap).

**Rung 3: Your AI Tools Become the Attack Surface (2025–2026)**

* **s1ngularity / Nx (August 2025):** attackers published poisoned versions of the popular Nx build tool. The malware ran AI coding assistants already installed on developers' machines (the command-line tools from Anthropic, Google and Amazon) with their safety prompts disabled, and told them to search for secrets. More than 1,700 users had secrets leaked, and in a second phase over 6,700 private repositories were made public. Interestingly, the AI-assisted theft succeeded in fewer than 25% of cases, partly because one of the assistants refused many requests.
* **DUSTMAKER (reported September 2026):** malware that plants prompt-injection files in the configuration folders that AI coding tools read (such as `.cursor/`), and embeds prompts aimed at AI-based security scanners.

**Rung 4: AI-Orchestrated Operations (2025–2026)**

* **August 2025:** Anthropic reported a criminal (GTG-2002) who used its coding agent to automate reconnaissance, credential theft and break-ins against at least 17 organizations, then demanded ransoms, sometimes above $500,000. The same report described North Korean IT workers using AI to get and keep remote jobs.
* **November 2025, the first reported AI-orchestrated espionage campaign:** Anthropic attributed, with high confidence, a campaign to a Chinese state-sponsored group (GTG-1002). It used Anthropic's coding agent with MCP tools against about 30 targets, with "a handful" of successful intrusions. Anthropic estimates the AI did 80–90% of the tactical work, with humans stepping in at a few decision points. The AI also frequently exaggerated or made up results, such as credentials that didn't work. Critics noted that no indicators of compromise were published, so the claims are hard to verify independently.
* **September 2026, Google's "From Prompting to Autonomy":** one attacker broke into a cloud account, then used an AI coding assistant plus written "playbooks" to plan and run a credential-harvesting campaign in under six hours, compromising thousands of credentials. Google also saw model-extraction attempts (trying to copy a model by querying it) exceeding 100 million prompts. Google says it has *not yet* seen fully autonomous attack pipelines used against real targets.
* **September 2026, Anthropic's report covering December 2025 – August 2026:** five cyber actors in which AI executed or orchestrated most of the operation while humans still chose targets and how to make money. One group scanned about 1.8 million Android apps for leaked secrets. Another tried to prompt-inject the evaluation sandboxes of about 30 AI companies to reach an unreleased model, and failed.

**Rung 5: AI-Discovered Exploits in the Wild (2026)**

* **May 2026:** Google's threat intelligence group reported, with high confidence, the first exploit for a new bug that was developed with AI: a two-factor-authentication bypass in an open-source web admin tool, held by criminal groups and stopped before planned mass exploitation. The tell-tale signs included a made-up CVSS score and tutorial-style comments.
* **October 2026:** Google analyzed an AI-discovered vulnerability (CVE-2026-1731) that was exploited within 4 days of disclosure.

**Myth-Busting: What Was Overhyped**

* **"PromptLock, the first AI ransomware" (August 2025):** it turned out to be an academic proof of concept from NYU ("Ransomware 3.0"), never seen attacking anyone.
* **"PROMPTFLUX rewrites itself with AI":** Google labels it experimental, and its self-modification function was commented out.
* **"Shai-Hulud used AI":** the September 2025 npm worm did not use AI coding tools as a weapon; only s1ngularity did. There is only a moderate-confidence assessment that an LLM wrote part of its script.
* **"AI phishing is 4.5× more effective" (Microsoft Digital Defense Report 2025):** this traces back to the 101-person lab study above, not to real-world telemetry.
* **"The first AI zero-day exploited in the wild" (May 2026):** it was intercepted while attackers were still planning, before mass exploitation.

**Detection Ideas for Defenders**

* **Watch for LLM API traffic from places it shouldn't come from:** servers and endpoints calling AI APIs, especially with hard-coded keys, are a new signal.
* **Treat AI coding agents as privileged software:** they can read secrets and run commands. Log them and limit their permissions.
* **Look for machine tempo:** agent-driven intrusions run at a speed and regularity humans can't match. That is useful for anomaly detection, and it connects directly to the [IDS](../IDS/) folder.
* **Verify identity out-of-band:** for any request involving money or credentials, call back on a known number.
* **Check for planted instructions:** unexpected files in AI-tool configuration folders deserve the same suspicion as a new cron job.

**Want to Dive Deeper?**

* [Google GTIG: Adversarial misuse of generative AI (January 2025)](https://cloud.google.com/blog/topics/threat-intelligence/adversarial-misuse-generative-ai)
* [Google GTIG: Threat actor usage of AI tools (November 2025)](https://cloud.google.com/blog/topics/threat-intelligence/threat-actor-usage-of-ai-tools)
* [Google GTIG: AI vulnerability exploitation for initial access (May 2026)](https://cloud.google.com/blog/topics/threat-intelligence/ai-vulnerability-exploitation-initial-access)
* [Google GTIG: From Prompting to Autonomy (September 2026)](https://cloud.google.com/blog/topics/threat-intelligence/from-prompting-to-autonomy-the-evolution-of-adversarial-ai)
* [CERT-UA advisory on LAMEHUG](https://cert.gov.ua/article/6284730)
* [Anthropic: Disrupting the first reported AI-orchestrated cyber espionage campaign](https://www.anthropic.com/news/disrupting-AI-espionage) and [a skeptical take](https://www.bleepingcomputer.com/news/security/anthropic-claims-of-claude-ai-automated-cyberattacks-met-with-doubt/amp/)
* [Anthropic threat intelligence report, September 2026](https://www.anthropic.com/threat-intelligence-report-september-2026?page=1)
* [Nx security advisory (s1ngularity)](https://github.com/nrwl/nx/security/advisories/GHSA-cxm3-wv7p-598c) · [Wiz: s1ngularity's aftermath](https://www.wiz.io/blog/s1ngularitys-aftermath)
* [Microsoft: AI vs. AI, detecting an AI-obfuscated phishing campaign](https://www.microsoft.com/en-us/security/blog/2025/09/24/ai-vs-ai-detecting-an-ai-obfuscated-phishing-campaign/)
* [Evaluating LLMs' capability to launch fully automated spear phishing campaigns](https://arxiv.org/abs/2412.00586)
* [Ransomware 3.0 (the PromptLock paper)](https://arxiv.org/abs/2508.20444)

Attackers adopted AI the same way defenders did: first as a helper, then as a tool-user, and now increasingly as an operator. The good news is that every rung of the ladder leaves new traces to detect.
