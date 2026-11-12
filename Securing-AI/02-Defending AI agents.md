## Defending AI Agents: Patterns, Standards and Honest Limits (2024–2026)

*Covers July 2024 – November 2026.*

If [01-Attacks on AI agents](01-Attacks%20on%20AI%20agents.md) made you nervous, good. This file covers what the field learned about defense. The most important lesson fits in one sentence: **you can't reliably filter prompt injection out, so design systems that stay safe even when the model is fooled.** It's the same idea as least privilege and sandboxing, applied to a new kind of employee.

**The Uncomfortable Truth: Filters Don't Hold**

* **The Attacker Moves Second (October 2025):** a 14-author team including Milad Nasr, Nicholas Carlini and Florian Tramèr attacked 12 published defenses against jailbreaks and prompt injection, using adaptive attacks (gradient search, reinforcement learning, LLM-guided search) plus a human red-team contest with more than 500 participants. Most defenses fell to attack success rates above 90%, and the human red-teamers beat every challenge. One defense went from 2% attack success on a static benchmark to 96% under adaptive search.
* **AI Agents May Always Fall for Prompt Injections (May 2026):** a theoretical argument that an attacker can always make a forbidden action look legitimate in context.
* **Takeaway:** a low score on a fixed benchmark says little about real robustness. Treat classifiers and "guard" models as one layer, never the only one.

**Design Patterns That Do Work**

* **Break the lethal trifecta:** remove at least one of private data, untrusted input or the ability to send data out.
* **Meta's "Agents Rule of Two" (October 2025):** in any single session, an agent should have at most *two* of these three: (A) processing untrusted input, (B) access to sensitive data or systems, (C) the ability to change things or communicate externally. If a task needs all three, require human approval or start a fresh context.
* **Design Patterns for Securing LLM Agents (June 2025):** a 14-author paper describing six reusable patterns:
  * **Action-Selector:** the agent may only choose from a fixed menu of actions.
  * **Plan-Then-Execute:** the plan is fixed before any untrusted data is read.
  * **LLM Map-Reduce:** untrusted documents are processed in isolated sub-calls.
  * **Dual LLM:** a privileged model never sees untrusted text; a quarantined model processes it but has no tools.
  * **Code-Then-Execute:** the model writes a program, and the program (not the model) touches the data.
  * **Context-Minimization:** remove the user's original prompt once it's no longer needed.
* **CaMeL (Google DeepMind, March 2025):** the most complete version of the Dual LLM idea. A privileged model turns the user's request into a small program. A quarantined model parses untrusted data. A custom interpreter tracks where every value came from and enforces security policies before each tool call. It solved 77% of tasks with provable security, against 84% with no defense, at roughly 2.7–2.8× the token cost. Full review in [Papers/08](../Papers/08-%20Defeating%20Prompt%20Injections%20by%20Design%20%28March%202025%29.md).

**Practical Checklist for Anyone Deploying an Agent**

* **Least privilege:** give tokens the narrowest scope possible, and never use "always allow" on tools that can send data out.
* **Human approval for dangerous actions:** sending messages, making payments, deleting things, pushing code.
* **Egress control:** restrict which domains the agent can reach, including image URLs.
* **Pin and review tools:** pin MCP server versions, review tool descriptions, and watch for changes (rug pulls).
* **Sandbox code execution:** containers or VMs with no access to credentials.
* **Log everything:** prompts, tool calls and outputs. These logs are your new IDS data source.
* **Plant canaries:** decoy tools and fake credentials that should never be touched (see [Honeypots/05](../Honeypots/05-Deception%20against%20AI%20attackers.md)).

**MCP Grew Up (a Little)**

The MCP specification hardened its authorization model over three revisions:

* **2025-06-18:** MCP servers became OAuth resource servers, and clients must send resource indicators so a malicious server can't obtain or reuse tokens meant for another.
* **2025-11-25:** a simpler, safer way for clients to register.
* **2026-07-28:** the protocol became stateless, and stricter token-issuer validation is required.

The Coalition for Secure AI (CoSAI) published an MCP threat taxonomy in January 2026 with about 40 threats in 12 categories. Trusting tool descriptions and launching local tools securely are still unsolved.

**The Standards Map (What to Cite in 2026)**

* **OWASP Top 10 for LLM Applications:** the **2025** edition (November 2024) added System Prompt Leakage and Vector & Embedding Weaknesses. It has now been replaced by the **2026** edition (August 2026), where Excessive Agency rises to #3. Prompt Injection has remained #1 throughout.
* **OWASP Top 10 for Agentic Applications (December 2025):** a separate list for agents, running from Agent Goal Hijack (ASI01) to Rogue Agents (ASI10).
* **OWASP Agent Control Standard v0.1 (September 2026):** runtime enforcement for agents, including telemetry and an "Agent Bill of Materials".
* **MITRE ATLAS:** the ATT&CK-style knowledge base for attacks on AI. It had 10 releases in 2026 and, as of v2026.10, contains 16 tactics, 129 techniques and 77 case studies. New entries include agent-tool poisoning, and case studies cover EchoLeak and postmark-mcp.
* **NIST:** the adversarial ML taxonomy [AI 100-2 E2025](https://csrc.nist.gov/pubs/ai/100/2/e2025/final) was finalized in March 2025. The Cyber AI Profile (IR 8596) is still a preliminary draft (December 2025), and an AI Agent Standards Initiative started in February 2026.
* **EU AI Act:** obligations for general-purpose AI models apply from August 2, 2025, supported by a voluntary Code of Practice with a Safety & Security chapter.

**Want to Dive Deeper?**

* [The Attacker Moves Second](https://arxiv.org/abs/2510.09023)
* [Defeating Prompt Injections by Design (CaMeL)](https://arxiv.org/abs/2503.18813)
* [Design Patterns for Securing LLM Agents against Prompt Injections](https://arxiv.org/abs/2506.08837)
* [Meta: Agents Rule of Two](https://ai.meta.com/blog/practical-ai-agent-security/)
* [AI Agents May Always Fall for Prompt Injections](https://arxiv.org/abs/2605.17634)
* [MCP specification changelog (2025-06-18)](https://modelcontextprotocol.io/specification/2025-06-18/changelog) · [(2026-07-28)](https://modelcontextprotocol.io/specification/2026-07-28/changelog)
* [OWASP GenAI Security Project: Agentic Top 10](https://genai.owasp.org/2025/12/09/owasp-genai-security-project-releases-top-10-risks-and-mitigations-for-agentic-ai-security/) · [LLM Top 10 2026](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/)
* [MITRE ATLAS data releases](https://github.com/mitre-atlas/atlas-data/releases)
* [NIST Cyber AI Profile (preliminary draft)](https://csrc.nist.gov/pubs/ir/8596/iprd) · [NIST AI Agent Standards Initiative](https://www.nist.gov/caisi/ai-agent-standards-initiative)
* [EU General-Purpose AI Code of Practice](https://digital-strategy.ec.europa.eu/en/policies/gpai-code-practice)

The honest state of the art: no filter stops prompt injection, but good architecture limits what a fooled agent can do. That's the same thing security engineering has always said about humans.
