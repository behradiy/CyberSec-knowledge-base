## Honeyval: A Comprehensive Evaluation Framework for LLM-powered HTTP Honeypots (May 2026)

[This paper](https://arxiv.org/abs/2605.29963), by Mark Vero, Fabian Kaczmarczyck, Ivan Petrov, Ilia Shumailov, Jamie Hayes, Niels Heinen, Tianqi Fan, Luca Invernizzi and Martin Vechev (ETH Zurich, Google and Google DeepMind), was posted on arXiv in May 2026 (v2 in November 2026) and presented as a poster at an ICML 2026 workshop. It answers the question every LLM-honeypot builder had been dodging: how do you know it actually works?

**The Challenge: Evaluating LLM Honeypots**

* LLM honeypots exploded after 2024 (see [04-LLM-powered honeypots](../Honeypots/04-LLM-powered%20honeypots.md)), but their evaluations were weak.
* Most compared responses to a fixed list of commands, relied on manual poking, or deployed on the Internet without a controlled baseline.
* None of these methods is scalable, reproducible, or representative of what a determined attacker actually does.

**The Proposed Solution: Honeyval**

* **Realistic targets:** each honeypot imitates one of 16 real backend web applications (taken from the BaxBench benchmark), so there is always a real application to compare against.
* **AI attackers:** the honeypots are attacked by AI hacking agents: a ReAct agent on Gemini 3 Flash or Anthropic's Sonnet 4.6, plus the command-line coding agents from Anthropic and Google.
* **Clear, verifiable goals:** each attack has a concrete objective, such as downloading a script, dropping a database table, changing the admin password or exfiltrating local storage.
* **Control tasks:** two control tasks check that both the attacker and the honeypot are capable, so a "win" isn't just a broken component.

**Key Findings**

* **Engagement:** LLM honeypots kept attackers busy for about 78.5 requests per session, against 30.7 for rule-based honeypots (about 2.6×).
* **Detection:** AI attackers judged LLM honeypots to be fake only about 24% of the time with the base prompt (mostly under 40% across models), while the rule-based Trapster honeypot was detected 97–99% of the time.
* **Cost:** on average the honeypot spent about 3.5× less than the agent attacking it.
* **Fidelity:** functional correctness (Pass@1) ranged from 71% to 97.5%, with Gemini 3 Flash the best honeypot model.
* **Trade-offs:** adding counter-attacks (prompt injections aimed at the attacker) shortened sessions to 49 requests and raised detection to 0.50. Caching cut latency but raised detection to 0.59. Latency was the attackers' most common reason for suspicion.

**Conclusion**

Honeyval gives the first solid, lab-based evidence that LLM honeypots engage attackers longer and fool them more often than rule-based ones, at a cost advantage. Just as important, it gives the community a reusable benchmark, which is exactly what the field lacked.

**As a Reviewer, My Focus**

* **No real-world deployment:** all attackers are coding agents given a specific target. Botnets, mass scanners and human operators may behave very differently. Pairing Honeyval with an Internet deployment (like Palisade's LLM Agent Honeypot) would show whether lab results transfer.
* **Narrow scope and a few inconsistencies:** only HTTP and small applications are covered. The honeypot implementation is naive (it resends the full context every time), and the cost model ignores tiered pricing. I also noticed the text mentions latencies above 33 seconds while the table reports a median of 3.3 seconds; this should be clarified.
* **How detection is measured:** "detection" means asking the agent for a verdict at the end. Some agents (Gemini CLI) also produce many false positives on *real* applications, and dedicated fingerprinting tools (such as the Rouxii agent) were not tested. As a workshop paper, it has also had a lighter review than a main-track publication.

**Further Exploration**

* [SoK: Honeypots & LLMs](https://arxiv.org/abs/2510.25939) (the systematization this work builds on)
* [ICML 2026 workshop poster page](https://icml.cc/virtual/2026/68342)
* [04-LLM-powered honeypots](../Honeypots/04-LLM-powered%20honeypots.md) in this repo
