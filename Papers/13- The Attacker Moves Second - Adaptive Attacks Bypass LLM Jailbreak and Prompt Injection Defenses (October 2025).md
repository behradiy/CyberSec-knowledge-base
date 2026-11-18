## The Attacker Moves Second: Stronger Adaptive Attacks Bypass Defenses Against LLM Jailbreaks and Prompt Injections (October 2025)

[This paper](https://arxiv.org/abs/2510.09023), by Milad Nasr, Nicholas Carlini, Chawin Sitawarin, Sander V. Schulhoff, Jamie Hayes and 9 co-authors (last author Florian Tramèr), brings together researchers from OpenAI, Google DeepMind, ETH Zürich, Northeastern University, HackAPrompt and other organizations. It was posted on arXiv in October 2025 as a preprint under review. Its message is simple and uncomfortable: almost every published defense against jailbreaks and prompt injection breaks once the attacker adapts to it.

**The Challenge: Defenses Tested Against Lazy Attackers**

* New defenses are usually evaluated against a fixed set of known attacks, or against generic, low-budget optimizers that aren't tuned to the defense.
* The adversarial-examples field learned the hard way that this gives a false sense of security: defenses that looked robust broke as soon as someone attacked them specifically.
* The authors argue that a robustness claim only means something if it survives an attacker who knows the defense and has real resources.

**The Approach: 12 Defenses vs. 4 Kinds of Adaptive Attack**

* **The defenses, in four families:**
    * Prompting: Spotlighting, Prompt Sandwiching, RPO.
    * Training: Circuit Breakers, StruQ, MetaSecAlign.
    * Filtering (detector models): Protect AI, PromptGuard, PIGuard, Model Armor.
    * Secret-knowledge: Data Sentinel, MELON.
* **The attacks:**
    * **Gradient-based:** GCG.
    * **Reinforcement learning:** an attacker LLM that reads the defense's paper and refines its attack over several rounds.
    * **Search-based:** an evolutionary search guided by an LLM critic, up to 800 queries.
    * **Human red-teaming:** an online competition with more than 500 participants, 40 challenges and $20,000 in prizes.
* **Benchmarks:** HarmBench for jailbreaks, AgentDojo (80 agent tasks) and OpenPromptInject for prompt injection.

**Key Findings**

* **Almost everything fell:** the attack success rate jumped from near zero to above 70% for every defense in the headline figure. For example:
    * Circuit Breakers went from 8% to 100%.
    * MetaSecAlign went from 5% to 96%.
    * RPO went from 0% to 99%.
    * PIGuard went from 0% to 71%.
    * MELON went from 0% to 89%.
* **Knowing the defense helps, but isn't required:** MELON was bypassed 76% of the time even when the attacker didn't know it was there, and 95% with full knowledge.
* **Humans beat every challenge:** across 29 scenarios that both humans and the automated search attempted, the human crowd succeeded on all of them, the automated search on 69% and the best single human on 75%.
* **The underlying model matters:** GPT-5 Mini was noticeably harder to break (47–56% with several defenses) than the model used for the headline numbers.
* **Defenses cost usefulness:** one filter reduced the agent's task success from 74% to 39%.

**Conclusion**

Near-zero attack rates on static benchmarks say very little about a motivated attacker. Prompt-based defenses, safety fine-tuning and classifier guardrails should be treated as speed bumps, not walls. The safer path is to design agents so that a successful injection *cannot* trigger harmful actions; see [Papers/08 (CaMeL)](08-%20Defeating%20Prompt%20Injections%20by%20Design%20%28March%202025%29.md) and [02-Defending AI agents](../Securing-AI/02-Defending%20AI%20agents.md).

**As a Reviewer, My Focus**

* **The headline figure shows the best case:** it uses results from the easiest-to-break model, while the tables show GPT-5 Mini holding up much better (47–81%). That weakens the abstract's claim of "above 90% for most" defenses. The figure and the text also disagree in places: RPO is 99% in the figure but 98% or 96% in the text; MELON 89% vs. 76% or 95%; and one detector's static rate is 0% in the figure but 28% in a table.
* **Thin coverage per defense:** each defense faces only one or two attack methods on a single small benchmark (80 tasks), without repeated runs or confidence intervals. The human "100%" means *at least one* participant succeeded; per submission, the success rate against MELON was only 0–2.4%. Both numbers matter, and the paper foregrounds only one.
* **A generous threat model, without costs:** the reinforcement-learning attacker is an unnamed model with no safety alignment that is given the defense paper itself, and MELON's 95% assumes full knowledge of the defense. Yet no compute or dollar costs are reported. Architectural defenses such as CaMeL were also excluded, so the conclusion "no defense is robust" applies only to the families tested.

**Further Exploration**

* [Defeating Prompt Injections by Design (CaMeL)](08-%20Defeating%20Prompt%20Injections%20by%20Design%20%28March%202025%29.md), the architectural alternative
* [Design Patterns for Securing LLM Agents against Prompt Injections](https://arxiv.org/abs/2506.08837)
* [AgentDojo](https://arxiv.org/abs/2406.13352), the agent benchmark used in the paper
