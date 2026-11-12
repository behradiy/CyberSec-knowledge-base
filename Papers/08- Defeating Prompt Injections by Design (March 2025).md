## Defeating Prompt Injections by Design (March 2025)

[This paper](https://arxiv.org/abs/2503.18813), by Edoardo Debenedetti, Ilia Shumailov, Tianqi Fan, Jamie Hayes, Nicholas Carlini, Daniel Fabian, Christoph Kern, Chongyang Shi, Andreas Terzis and Florian Tramèr (Google, Google DeepMind and ETH Zurich), was posted on arXiv in March 2025 and updated in June 2025. It introduces **CaMeL**, the most influential "secure by design" defense against prompt injection, and the code is [open source](https://github.com/google-research/camel-prompt-injection).

**The Challenge: Agents Read Untrusted Data**

* AI agents that read emails, documents or web pages can be hijacked by instructions hidden in that data (prompt injection; see [01-Attacks on AI agents](../Securing-AI/01-Attacks%20on%20AI%20agents.md)).
* Most defenses try to *detect* malicious text or *train* models to ignore it. Both can be bypassed by a determined, adaptive attacker.

**The Proposed Solution: Separate Control Flow from Data Flow**

CaMeL borrows ideas from classic software security (control-flow integrity, capabilities, information-flow control):

* **A privileged LLM** sees only the user's trusted request and writes a small Python program that describes the plan. It never sees untrusted data.
* **A quarantined LLM** parses untrusted data (an email, a document) into structured values. It has no access to tools.
* **A custom interpreter** runs the plan and tags every value with "capabilities": where it came from and who may see it. Before every tool call, it checks security policies (for example: "never send a document to an email address that came from untrusted data").
* **The result:** injected text can change *values*, but never the *program flow*, and private data can't leave through unauthorized channels.

**Key Findings**

* **Security with utility:** on the AgentDojo benchmark, CaMeL solved **77%** of tasks with provable security, against **84%** with no defense at all.
* **Attacks nearly eliminated:** successful attacks dropped to near zero (for one model, from 300 successful attacks to 0).
* **Cost:** the median task used about 2.8× more input tokens and 2.7× more output tokens.

**Conclusion**

CaMeL shows that you don't have to win the arms race of detecting malicious prompts. If the architecture guarantees that untrusted data can't change what the agent does, the model can be fooled and the system still stays safe. It inspired many of the design patterns now recommended for building agents (see [02-Defending AI agents](../Securing-AI/02-Defending%20AI%20agents.md)).

**As a Reviewer, My Focus**

* **Policies are the weak link:** CaMeL's guarantees are only as good as the security policies, which are written by hand. Too strict, and users drown in confirmation requests and start approving everything (confirmation fatigue). I'd like to see a study of how real users write and maintain these policies.
* **What it does not cover:** text-to-text manipulation (a poisoned summary or a phishing message the agent repeats), side channels such as exceptions or timing, and chaining individually allowed steps into a harmful sequence (similar to return-oriented programming) are out of scope. A clearer threat-model table would help practitioners understand the residual risk.
* **Limited evaluation:** it is tested only on AgentDojo, and the numbers changed noticeably between v1 and v2. Open-ended tasks, where the plan itself depends on untrusted data (coding, browsing), remain largely untested, and those are exactly the tasks people use agents for most.

**Further Exploration**

* [Design Patterns for Securing LLM Agents against Prompt Injections](https://arxiv.org/abs/2506.08837) (generalizes the idea into six patterns)
* [The Attacker Moves Second](https://arxiv.org/abs/2510.09023) (why detection-based defenses fail)
* [Meta's Agents Rule of Two](https://ai.meta.com/blog/practical-ai-agent-security/)
