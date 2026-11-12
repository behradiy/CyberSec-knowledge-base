<a id="readme-top"></a>

<div align="center">

# 🛡️ CyberSec Knowledge Base

### Where Cybersecurity Meets Artificial Intelligence

An open, growing knowledge base on honeypots, intrusion detection, AI-powered attack and defense, and securing AI systems.<br>
Born from my B.Sc. final project at the University of Tabriz. Fully updated for 2024–2026.

[![Last commit](https://img.shields.io/github/last-commit/behradiy/CyberSec-knowledge-base?style=flat-square&label=last%20update)](https://github.com/behradiy/CyberSec-knowledge-base/commits/main)
[![Articles](https://img.shields.io/badge/articles-14-2ea44f?style=flat-square)](#contents)
[![Paper reviews](https://img.shields.io/badge/paper%20reviews-9-8957e5?style=flat-square)](Papers/)
[![Timeline](https://img.shields.io/badge/timeline-Jul%202024%20%E2%86%92%20Nov%202026-0969da?style=flat-square)](Timeline%202024-2026.md)
[![Stars](https://img.shields.io/github/stars/behradiy/CyberSec-knowledge-base?style=flat-square)](https://github.com/behradiy/CyberSec-knowledge-base/stargazers)

<p><b>
<a href="Honeypots/">🍯 Honeypots</a> ·
<a href="IDS/">🐕 IDS</a> ·
<a href="AI-Attack-and-Defense/">⚔️ AI Attack &amp; Defense</a> ·
<a href="Securing-AI/">🤖 Securing AI</a> ·
<a href="Papers/">📄 Papers</a> ·
<a href="Timeline%202024-2026.md">🗓️ Timeline</a>
</b></p>

<p><sub>
<a href="#start">Start here</a> ·
<a href="#map">Map</a> ·
<a href="#whats-new">What's new</a> ·
<a href="#paths">Learning paths</a> ·
<a href="#contents">All contents</a> ·
<a href="#contribute">Contribute</a> ·
<a href="#about">About</a>
</sub></p>

</div>

---

<a id="start"></a>

## 🧭 Start Here: What Do You Want to Do?

| I want to... | Go to |
|---|---|
| 🍯 Understand what a honeypot is | [Honeypots: the basics](Honeypots/01-what%20are%20them.md) |
| 🐕 Learn how intrusion detection works | [What is an IDS?](IDS/01-what%20is%20that.md) |
| 🧑‍💻 Become an IDS developer | [What should you learn](IDS/02-what%20should%20you%20learn.md) → [Network fundamentals roadmap](IDS/0201-Network%20Fundamentals.md) |
| 🗓️ Catch up on 2024–2026 in ten minutes | [Timeline 2024-2026](Timeline%202024-2026.md) |
| 🤖 Build a honeypot powered by an LLM | [LLM-powered honeypots](Honeypots/04-LLM-powered%20honeypots.md) |
| 🕵️ Trap AI hacking agents | [Deception against AI attackers](Honeypots/05-Deception%20against%20AI%20attackers.md) |
| 🐞 See how AI finds and fixes zero-days | [AI bug hunters](AI-Attack-and-Defense/01-AI%20bug%20hunters.md) |
| 🦹 Know how attackers really use AI | [How attackers use AI](AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md) |
| 🔐 Secure my AI agent or chatbot | [Attacks on AI agents](Securing-AI/01-Attacks%20on%20AI%20agents.md) → [Defending AI agents](Securing-AI/02-Defending%20AI%20agents.md) |
| 📄 Read critical reviews of research papers | [Paper reviews](Papers/) |

<a id="map"></a>

## 🗺️ The Map

<table>
<tr>
<td width="50%" valign="top">

### 🍯 [Honeypots](Honeypots/)

Decoy systems that lure attackers in and study them, from classic traps to honeypots that improvise with an LLM and decoys built to fool AI agents.

**5 articles** · `NEW` LLM honeypots

</td>
<td width="50%" valign="top">

### 🐕 [IDS](IDS/)

Intrusion detection from the ground up: how it works, what to learn, a networking roadmap, and how AI changed detection and the SOC.

**5 articles** · `NEW` agentic SOC

</td>
</tr>
<tr>
<td width="50%" valign="top">

### ⚔️ [AI Attack & Defense](AI-Attack-and-Defense/)

AI systems that find and fix vulnerabilities, and what threat-intelligence teams saw attackers actually do with AI.

**2 articles** · `NEW` section

</td>
<td width="50%" valign="top">

### 🤖 [Securing AI](Securing-AI/)

Prompt injection, poisoned tools and hijacked agents, plus the design patterns and standards that defend against them.

**2 articles** · `NEW` section

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 📄 [Papers](Papers/)

Summaries and critical reviews of research papers, from a 2004 honeypot design to 2026 benchmarks, plus a reading list.

**9 reviews** · `NEW` 04–09

</td>
<td width="50%" valign="top">

### 🗓️ [Timeline](Timeline%202024-2026.md)

Everything important that happened in cybersecurity from July 2024 to November 2026, month by month, each event linked to its source.

**~120 events** · `NEW`

</td>
</tr>
</table>

<a id="whats-new"></a>

## 🆕 What's New: The November 2026 Update

This repository was quiet from July 2024 to November 2026, and the field did not wait. AI went from a promising research topic to something that finds real zero-days, runs real attacks and defends real networks. This update catches up on all of it. Every recent claim is linked to its source, and claims that only a vendor has made are labeled as such.

**The three biggest shifts since 2024:**

1. **AI finds real vulnerabilities at scale.** From DARPA's AI Cyber Challenge (2025) to frontier models finding thousands of zero-days (2026), discovery is no longer the bottleneck. Patching is. → [AI bug hunters](AI-Attack-and-Defense/01-AI%20bug%20hunters.md)
2. **Attackers use AI in real operations.** Threat-intelligence teams documented malware that queries LLMs, AI-orchestrated intrusions and the first AI-built exploits in the wild. → [How attackers use AI](AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md)
3. **AI systems became an attack surface.** Prompt injection, poisoned tools and hijacked agents are now core security work, and honeypots adapted to trap AI attackers too. → [Securing AI](Securing-AI/) · [Deception against AI attackers](Honeypots/05-Deception%20against%20AI%20attackers.md)

<details>
<summary><b>📰 Six headlines worth knowing (click to expand)</b></summary>
<br>

* **Autonomous bug hunting works:** at DARPA's AI Cyber Challenge final (August 2025), AI systems found 54 of 63 planted vulnerabilities and 18 real ones. → [AI bug hunters](AI-Attack-and-Defense/01-AI%20bug%20hunters.md)
* **The "98% accurate" AI traffic classifiers mostly learned shortcuts:** a proper evaluation dropped them to 11–16%. → [AI meets network detection](IDS/03-AI%20meets%20network%20detection.md)
* **LLM honeypots fool AI attackers:** in the first rigorous benchmark, attacking agents spotted them only about 24% of the time, against 97–99% for a rule-based honeypot. → [LLM-powered honeypots](Honeypots/04-LLM-powered%20honeypots.md)
* **The first AI-orchestrated espionage campaign was reported** in November 2025. → [How attackers use AI](AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md)
* **No prompt-injection filter survives adaptive attacks**, so safe AI agents are built by design, not by filtering. → [Defending AI agents](Securing-AI/02-Defending%20AI%20agents.md)
* **RSA and elliptic-curve crypto now have an expiry date:** NIST deprecates them after 2030 and disallows them after 2035. → [Paper 09](Papers/09-%20How%20to%20Factor%202048%20bit%20RSA%20Integers%20with%20Less%20than%20a%20Million%20Noisy%20Qubits%20%28May%202025%29.md)

</details>

<a id="paths"></a>

## 🎓 Learning Paths

Not sure where to begin? Pick the path that fits you and follow it top to bottom.

<table>
<tr>
<th width="33%">🌱 Beginner</th>
<th width="33%">🔧 Practitioner</th>
<th width="33%">🔬 Researcher</th>
</tr>
<tr>
<td valign="top">

1. [Honeypots: the basics](Honeypots/01-what%20are%20them.md)
2. [What is an IDS?](IDS/01-what%20is%20that.md)
3. [What should you learn](IDS/02-what%20should%20you%20learn.md)
4. [Network fundamentals roadmap](IDS/0201-Network%20Fundamentals.md)
5. [Intelligent honeypots](Honeypots/02-Intelligentones.md)
6. [Honeypots & AI](Honeypots/03-integration%20with%20AI.md)
7. [Timeline 2024-2026](Timeline%202024-2026.md)

</td>
<td valign="top">

1. [LLM-powered honeypots](Honeypots/04-LLM-powered%20honeypots.md)
2. [Deception against AI attackers](Honeypots/05-Deception%20against%20AI%20attackers.md)
3. [AI meets network detection](IDS/03-AI%20meets%20network%20detection.md)
4. [The agentic SOC](IDS/04-The%20agentic%20SOC.md)
5. [How attackers use AI](AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md)
6. [Attacks on AI agents](Securing-AI/01-Attacks%20on%20AI%20agents.md)
7. [Defending AI agents](Securing-AI/02-Defending%20AI%20agents.md)

</td>
<td valign="top">

1. [Dynamic honeypot design (2004)](Papers/01-%20A%20Dynamic%20Honeypot%20Design%20for%20Intrusion%20Detection%20%28July%202004-%20review%20paper%29.md)
2. [RL dynamic honeypots for IoBT (2021)](Papers/03-%20Reinforcement%20Learning-assisted%20Threshold%20Optimization%20for%20Dynamic%20Honeypot%20Adaptation%20to%20Enhance%20IoBT%20Networks%20Security%20%28December%202021%29.md)
3. [Cloak, Honey, Trap (2025)](Papers/04-%20Cloak%2C%20Honey%2C%20Trap%20-%20Proactive%20Defenses%20Against%20LLM%20Agents%20%28August%202025%29.md)
4. [Honeyval (2026)](Papers/05-%20Honeyval%20-%20A%20Comprehensive%20Evaluation%20Framework%20for%20LLM-powered%20HTTP%20Honeypots%20%28May%202026%29.md)
5. [The Sweet Danger of Sugar (2025)](Papers/06-%20The%20Sweet%20Danger%20of%20Sugar%20-%20Debunking%20Representation%20Learning%20for%20Encrypted%20Traffic%20Classification%20%28July%202025%29.md)
6. [SoK: DARPA's AIxCC (2026)](Papers/07-%20SoK%20-%20DARPA%27s%20AI%20Cyber%20Challenge%20%28AIxCC%29%20%28February%202026%29.md)
7. [The reading list](Papers/README.md)

</td>
</tr>
</table>

<a id="contents"></a>

## 📚 All Contents

Click a section to see every article in it.

<details>
<summary><b>🍯 Honeypots</b> · 5 articles</summary>
<br>

| # | Article | What you'll learn |
|---|---|---|
| 01 | [What are honeypots?](Honeypots/01-what%20are%20them.md) | The basics: types of honeypots, honeynets, spam traps and their limits |
| 02 | [Intelligent honeypots](Honeypots/02-Intelligentones.md) | Honeypots that learn and adapt, and the challenges they face |
| 03 | [Honeypots & AI](Honeypots/03-integration%20with%20AI.md) | Why combining honeypots with AI makes both stronger |
| 04 | [LLM-powered honeypots](Honeypots/04-LLM-powered%20honeypots.md) `NEW` | Galah, Beelzebub, T-Pot, DECEIVE, Cowrie's LLM mode, and the evidence on whether they work |
| 05 | [Deception against AI attackers](Honeypots/05-Deception%20against%20AI%20attackers.md) `NEW` | Catching AI hacking agents, MCP canary tokens and the deception arms race |

</details>

<details>
<summary><b>🐕 Intrusion Detection (IDS)</b> · 5 articles</summary>
<br>

| # | Article | What you'll learn |
|---|---|---|
| 01 | [What is an IDS?](IDS/01-what%20is%20that.md) | NIDS vs. HIDS, how detection works and where it falls short |
| 02 | [What should you learn](IDS/02-what%20should%20you%20learn.md) | The skills needed to build intrusion detection systems |
| 0201 | [Network fundamentals roadmap](IDS/0201-Network%20Fundamentals.md) | A phased plan with courses, books and practice tools |
| 03 | [AI meets network detection](IDS/03-AI%20meets%20network%20detection.md) `NEW` | Shortcut learning in AI traffic models, dataset problems, LLMs as detectors, Suricata 8 and Zeek 9 |
| 04 | [The agentic SOC](IDS/04-The%20agentic%20SOC.md) `NEW` | AI agents that triage alerts, what vendors shipped and what the evidence shows |

</details>

<details>
<summary><b>⚔️ AI Attack &amp; Defense</b> · 2 articles</summary>
<br>

| # | Article | What you'll learn |
|---|---|---|
| 01 | [AI bug hunters](AI-Attack-and-Defense/01-AI%20bug%20hunters.md) `NEW` | Big Sleep, DARPA AIxCC, frontier models finding zero-days, benchmarks, and the patching bottleneck |
| 02 | [How attackers use AI](AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md) `NEW` | Documented real-world cases, myth-busting, and detection ideas for defenders |

</details>

<details>
<summary><b>🤖 Securing AI</b> · 2 articles</summary>
<br>

| # | Article | What you'll learn |
|---|---|---|
| 01 | [Attacks on AI agents](Securing-AI/01-Attacks%20on%20AI%20agents.md) `NEW` | Prompt injection, the "lethal trifecta", real CVEs, MCP supply-chain attacks and model poisoning |
| 02 | [Defending AI agents](Securing-AI/02-Defending%20AI%20agents.md) `NEW` | Why filters fail, design patterns that work, a deployment checklist and the standards map |

</details>

<details>
<summary><b>📄 Paper Reviews</b> · 9 reviews</summary>
<br>

| # | Paper | Topic | Year |
|---|---|---|---|
| 01 | [A Dynamic Honeypot Design for Intrusion Detection](Papers/01-%20A%20Dynamic%20Honeypot%20Design%20for%20Intrusion%20Detection%20%28July%202004-%20review%20paper%29.md) | Honeypots | 2004 |
| 02 | [Machine Learning and Cybersecurity: Hype and Reality](Papers/02-%20Machine-Learning-and-Cybersecurity.md) | AI & security | 2021 |
| 03 | [RL-assisted Dynamic Honeypot Adaptation for IoBT Networks](Papers/03-%20Reinforcement%20Learning-assisted%20Threshold%20Optimization%20for%20Dynamic%20Honeypot%20Adaptation%20to%20Enhance%20IoBT%20Networks%20Security%20%28December%202021%29.md) | Honeypots & RL | 2021 |
| 04 | [Cloak, Honey, Trap: Proactive Defenses Against LLM Agents](Papers/04-%20Cloak%2C%20Honey%2C%20Trap%20-%20Proactive%20Defenses%20Against%20LLM%20Agents%20%28August%202025%29.md) `NEW` | Deception vs. AI attackers | 2025 |
| 05 | [Honeyval: Evaluating LLM-powered HTTP Honeypots](Papers/05-%20Honeyval%20-%20A%20Comprehensive%20Evaluation%20Framework%20for%20LLM-powered%20HTTP%20Honeypots%20%28May%202026%29.md) `NEW` | LLM honeypots | 2026 |
| 06 | [The Sweet Danger of Sugar](Papers/06-%20The%20Sweet%20Danger%20of%20Sugar%20-%20Debunking%20Representation%20Learning%20for%20Encrypted%20Traffic%20Classification%20%28July%202025%29.md) `NEW` | IDS & traffic ML | 2025 |
| 07 | [SoK: DARPA's AI Cyber Challenge (AIxCC)](Papers/07-%20SoK%20-%20DARPA%27s%20AI%20Cyber%20Challenge%20%28AIxCC%29%20%28February%202026%29.md) `NEW` | AI vulnerability discovery | 2026 |
| 08 | [Defeating Prompt Injections by Design (CaMeL)](Papers/08-%20Defeating%20Prompt%20Injections%20by%20Design%20%28March%202025%29.md) `NEW` | Securing AI agents | 2025 |
| 09 | [How to Factor 2048 bit RSA with Less than a Million Noisy Qubits](Papers/09-%20How%20to%20Factor%202048%20bit%20RSA%20Integers%20with%20Less%20than%20a%20Million%20Noisy%20Qubits%20%28May%202025%29.md) `NEW` | Post-quantum cryptography | 2025 |

More strong papers that are waiting for a review are in the [reading list](Papers/README.md).

</details>

<details>
<summary><b>🗓️ Timeline</b> · July 2024 → November 2026</summary>
<br>

[Timeline 2024-2026](Timeline%202024-2026.md): about 120 events, month by month, each tagged by topic (🍯 deception · 🛡️ detection · 🐞 AI bug hunting · 🦹 attackers using AI · 🤖 securing AI · 🔐 cryptography · 🌍 incidents & policy) and linked to its source.

</details>

<p align="right"><a href="#readme-top">⬆ back to top</a></p>

<a id="contribute"></a>

## 🤝 Contribute

I believe open-source collaboration is essential for building the cybersecurity solutions of tomorrow. Working together, we can:

* **Unify knowledge:** gather insights from different perspectives into one organized place.
* **Accelerate innovation:** share ideas, code and research findings on AI-powered security.
* **Bridge the gap:** connect academic research with real-world practice.

**Ways to help:**

| You found... | Do this |
|---|---|
| 🐛 A mistake or an outdated claim | [Open an issue](https://github.com/behradiy/CyberSec-knowledge-base/issues) with the correct source |
| ✍️ A topic worth an article | Open a pull request with a numbered file in the right folder, and link every claim to a source |
| 📄 A paper worth reviewing | Follow the format in [Papers](Papers/): summary, key findings and an "As a Reviewer, My Focus" critique |
| 🗓️ A missing event | Add one line to the [Timeline](Timeline%202024-2026.md) with a primary source |

<a id="about"></a>

## 👋 About

Hi, I'm **Behrad Azimi**. This repository started as my final project for the B.Sc. in Computer Science at the **University of Tabriz**, on AI-driven honeypots for threat detection, and grew into a knowledge base on how AI is changing cybersecurity, for students, practitioners and researchers alike.

* 🌐 Website: [behradazimi.dev](https://behradazimi.dev)
* 💬 Telegram: [@B3hrady](https://t.me/B3hrady). Questions, ideas, or just say hello 😄

> ⚖️ **Use responsibly.** Everything here is for education and defense. Follow the law and ethical guidelines when deploying honeypots or testing systems, and only test systems you are authorized to test.

---

<div align="center">

**The field of cybersecurity never stops evolving, and neither should we.**<br>
Let's build a stronger, more secure cyberspace, together.

⭐ If this knowledge base helped you, consider giving it a star.

<a href="#readme-top">⬆ back to top</a>

</div>
