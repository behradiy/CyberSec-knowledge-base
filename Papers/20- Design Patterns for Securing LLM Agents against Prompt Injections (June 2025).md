## Design Patterns for Securing LLM Agents against Prompt Injections (June 2025)

[This paper](https://arxiv.org/abs/2506.08837), by Luca Beurer-Kellner, Beat Buesser, Ana-Maria Creţu, Edoardo Debenedetti and 10 co-authors (authors listed alphabetically, corresponding author Florian Tramèr), brings together researchers from Invariant Labs, IBM, EPFL, ETH Zurich, Swisscom, Google, Microsoft and others. It was posted on arXiv in June 2025 as a 32-page preprint. Instead of another filter, it offers something closer to a software-engineering handbook: six ways to build agents so that a prompt injection *can't* make them do anything harmful.

**The Challenge: You Can't Filter Your Way Out**

* Prompt injection lets any text the model reads take over the agent, leading to data theft, privilege escalation or even remote code execution.
* Defenses inside the model (better prompts, adversarial training) and detector models are heuristic: they "cannot guarantee prevention of all attacks".
* Asking a human to confirm every action defeats the point of automation, and tired reviewers approve unsafe actions anyway.
* The authors consider it unlikely that *general-purpose* agents can offer reliable guarantees with today's LLMs. So they ask a narrower question: what can we guarantee for agents built for one specific job?

**The Core Principle**

Once an agent has read untrusted input, it must be constrained so that this input cannot trigger any consequential action. Its output must not create risks either, for example by leaking data through a link.

**The Six Patterns**

* **Action-Selector:** the model maps the request to one or more predefined (possibly templated) actions, and the actions' outputs are never fed back to it. Think of an LLM-driven `switch` statement.
* **Plan-Then-Execute:** the model fixes a plan before it reads any untrusted data. Tool outputs can come back, but they can't change *which* actions run. Injected data can still change the *arguments* of the planned calls.
* **LLM Map-Reduce:** each untrusted document goes to an isolated LLM call that has no tools and returns a constrained output (such as yes/no). Code, or an LLM that only sees those outputs, combines the results.
* **Dual LLM** (first proposed by Simon Willison in 2023): a privileged model plans and uses tools but never sees untrusted text. A quarantined model reads that text but has no tools, and its results are passed around only as references.
* **Code-Then-Execute:** the model writes a program, which may call tools and quarantined LLMs, *before* seeing the data; the program then runs on the data. The paper points to CaMeL ([Papers/08](08-%20Defeating%20Prompt%20Injections%20by%20Design%20%28March%202025%29.md)) as an instance of this pattern.
* **Context-Minimization:** unnecessary content, including the user's original prompt, is removed from the context once it is no longer needed. This targets injections that come from the user.

**Ten Case Studies**

The patterns are applied to ten realistic agents: an OS assistant with fuzzy file search, a SQL agent, an email and calendar assistant, a customer-service chatbot, a booking assistant, a product recommender, a resume-screening assistant, a medication-leaflet chatbot, a medical-diagnosis intermediary and a software-engineering agent. A table maps each one to the patterns it uses.

**Key Findings**

* **Security costs flexibility, by design:** the patterns "explicitly prevent them from solving arbitrary tasks". The OS assistant loses most of the benefit of fuzzy search, the SQL agent can no longer judge whether its data is enough, and the coding agent only sees API descriptions.
* **Only one pattern is fully immune:** Action-Selector is "trivially immune" because the model never looks at data. Plan-Then-Execute protects the control flow, a kind of control-flow integrity, but "does not prevent all prompt injections".
* **Data can still be manipulated:** under Plan-Then-Execute or Code-Then-Execute, an injection can still rewrite the body of an email the plan was going to send, or hide sensitive data in it with invisible characters. The quarantined model in Dual LLM can still be injected and return tampered text.
* **The patterns limit the damage rather than remove it:** a malicious resume or product review can still promote *itself*, but it can't affect the others.
* **Detectors only raise the bar:** an attacker must fool both the agent and the detector, but detection remains "fundamentally heuristic". A topic classifier, for example, can be beaten by mixing an allowed and a forbidden question in one prompt.
* **Recommendations:** build application-specific agents with clear trust boundaries, and combine patterns, because "no single pattern is likely to suffice".

**Conclusion**

General-purpose agents can't be secured with today's LLMs, but agents built for one job can be, if their design makes it impossible for untrusted input to trigger consequential actions. This is least privilege and control-flow integrity applied to LLMs, and it is the same lesson [Papers/13](13-%20The%20Attacker%20Moves%20Second%20-%20Adaptive%20Attacks%20Bypass%20LLM%20Jailbreak%20and%20Prompt%20Injection%20Defenses%20%28October%202025%29.md) reached from the attacker's side: that paper skipped plan-then-execute defenses because its attacks would fail against them by construction. For the practical checklist, see [02-Defending AI agents](../Securing-AI/02-Defending%20AI%20agents.md).

**As a Reviewer, My Focus**

* **"Provable" is promised but never shown:** the abstract speaks of "provable resistance" and of systematically analyzing the patterns, yet the paper contains no formal security definitions, no proofs, no implementation and no experiments. The case-study judgments are qualitative and often hedged ("would probably", "presumably"). The patterns are convincing, but a reader should treat them as well-argued design guidance, not as verified guarantees.
* **Residual risk and utility are described but never measured:** apart from Action-Selector, the patterns protect *which* actions run but not *what data* flows into them, as the authors themselves concede. There is no table of which attacker goals each pattern stops, and no measurement of how much usefulness each one costs. CaMeL's evaluation (77% vs. 84% task success) shows such numbers can be collected.
* **Combining patterns is recommended but not analyzed:** the paper says to combine patterns but doesn't explain whether guarantees survive composition. Several case studies also fall back on user confirmation or classifiers, the very defenses the paper calls fatigue-prone or heuristic. The naming is inconsistent too: the SQL-agent section is headed "Plan-Then-Execute" but its text describes "the code-then-execute pattern", and it invokes an "Ask-The-User" pattern and "Action-Sandboxing", neither of which is among the six.

**Further Exploration**

* [Simon Willison's commentary on this paper](https://simonwillison.net/2025/Jun/13/prompt-injection-design-patterns/) and [the original Dual LLM pattern (2023)](https://simonwillison.net/2023/Apr/25/dual-llm-pattern/)
* [Code samples for the six patterns](https://github.com/ReversecLabs/design-patterns-for-securing-llm-agents-code-samples) (third-party, not by the authors)
* [Defeating Prompt Injections by Design (CaMeL)](08-%20Defeating%20Prompt%20Injections%20by%20Design%20%28March%202025%29.md), the evaluated system behind Code-Then-Execute
