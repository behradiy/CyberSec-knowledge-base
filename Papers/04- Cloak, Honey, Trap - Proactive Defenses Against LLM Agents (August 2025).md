## Cloak, Honey, Trap: Proactive Defenses Against LLM Agents (August 2025)

[This paper](https://www.usenix.org/conference/usenixsecurity25/presentation/ayzenshteyn), by Daniel Ayzenshteyn, Roy Weiss and Yisroel Mirsky (Ben-Gurion University of the Negev), was published at the 34th USENIX Security Symposium (pp. 8095–8114). It asks a question that would have sounded like science fiction when this repository started: if attackers send AI agents to hack us, can we exploit the weaknesses of the agents themselves?

**The Challenge: AI Pentest Agents Are Getting Good**

* Open-source LLM agents such as PentestGPT, AutoAttacker and PenHeal can scan, exploit and move through networks with little human help.
* Traditional defenses were designed for human attackers or scripted bots, not for an attacker that *reads and reasons* about everything it sees.

**The Insight: Agents Have Built-In Weaknesses**

* **They trust what they read:** text in a file, banner or web page can redirect them (prompt injection).
* **They have short memory:** PentestGPT, for example, keeps only its last 5 commands in context, so it can be led in circles.
* **They see text differently from humans:** tokenization quirks and invisible Unicode characters can hide content from humans but not from the model (or the other way around).

**The Proposed Solution: 6 Strategies, 15 Techniques**

The authors organize their defenses into six strategies and 15 concrete techniques. Examples include:

* **LLM-specific honeytokens:** credentials and hints that an LLM notices and pursues, but a human administrator wouldn't.
* **Loop traps:** content that sends the agent around in circles, exploiting its short memory.
* **Search-space explosion:** making the agent believe there are thousands of promising paths to explore.
* **Inducing the agent to run defender code:** getting the attacker's own agent to execute something that reveals or stops it.

They release **CHeaT**, an open-source tool that plants these defenses automatically.

**Key Findings**

* **Tested models:** GPT-4o, Anthropic's Sonnet 3.5, Gemini 1.5 Pro and Llama-3.1-70B.
* **Average defense success rate:** 55–67% across all technique and data-point combinations, and 100% for the best single combination.
* **Per agent:** 79% against AutoAttacker, 84% against PenHeal and 83% against PentestGPT.
* **End to end:** with the defenses on, PentestGPT running on GPT-4o got initial access on **0 of 33 runs** (11 CTF machines, 3 runs each).
* **Adaptive attackers:** an ensemble detector built to spot the defenses cut their success to 14.19%, but at a 25.07% false-positive rate, which would make the attacker's agent unusable in practice.

**Conclusion**

This paper turns the honeypot idea inside out. Instead of building a convincing fake *system*, it builds content that is irresistible or confusing specifically to an AI *reader*. For anyone who studied honeypots before 2024, it is the clearest bridge between classic deception and the age of AI attackers.

**As a Reviewer, My Focus**

Coming from a background in AI-driven honeypots, I find the framework very valuable, but three points deserve more attention:

* **The attackers are already dated:** the evaluation uses 2024-era research agents and models. In 2026, deception-aware agents such as Rouxii identified honeypots 97% of the time. A follow-up should test against agents that *expect* deception and are trained to resist it.
* **The end-to-end result is small:** "0 of 33" rests on 11 CTF machines with 3 runs each, and some of those machines were never compromised even *without* the defenses. That inflates the headline. A larger, more realistic testbed with a proper baseline per machine would make the result much stronger.
* **Side effects and ethics are not measured:** there is no study of how these traps affect legitimate users, administrators or benign automation in a real network. Making an attacker's agent execute defender code also carries legal and ethical risk (it resembles hack-back), which deserves a dedicated discussion.

**Further Exploration**

* [Mantis: Hacking Back the AI-Hacker](https://arxiv.org/abs/2410.20911) (an earlier, related idea)
* [Rouxii: Deception-Aware AI Pentesters](https://arxiv.org/abs/2609.26555) (the attacker's answer)
* [05-Deception against AI attackers](../Honeypots/05-Deception%20against%20AI%20attackers.md) in this repo
