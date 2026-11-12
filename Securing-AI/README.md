# Securing AI

The `Securing-AI` folder is about protecting AI systems themselves: chatbots, coding assistants and autonomous agents that read data, call tools and act on our behalf. Since 2024, these systems have become a major new attack surface, and securing them is now part of every security engineer's job.

**Content**

* [01-Attacks on AI agents](01-Attacks%20on%20AI%20agents.md) - Prompt injection explained, the "lethal trifecta", real incidents and CVEs (EchoLeak, the GitHub MCP exploit, agentic browsers), MCP supply-chain attacks, model poisoning, and agents escaping their evaluation sandboxes.
* [02-Defending AI agents](02-Defending%20AI%20agents.md) - Why filters alone fail, the design patterns that work (Rule of Two, Dual LLM, CaMeL), a deployment checklist, and a map of the standards (OWASP, MITRE ATLAS, NIST, EU AI Act).

**Key Terms**

* **Prompt injection:** hiding instructions in data so an AI follows them.
* **MCP (Model Context Protocol):** the common standard for connecting tools to AI agents.
* **Agent:** an LLM that can take actions (run code, send messages, call APIs), not just chat.

Related folders: [AI-Attack-and-Defense](../AI-Attack-and-Defense/) (AI used by attackers and defenders) and [Honeypots](../Honeypots/) (decoys and canaries for AI agents).

**Contribute! This field moves monthly. If you find a new incident, CVE or defense paper, open a pull request with a link to the primary source.**
