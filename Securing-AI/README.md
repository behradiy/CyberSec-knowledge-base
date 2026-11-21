# Securing AI

The `Securing-AI` folder is about protecting AI systems themselves: chatbots, coding assistants and autonomous agents that read data, call tools and act on our behalf. Since 2024, these systems have become a major new attack surface, and securing them is now part of every security engineer's job.

**Content**

* [01-Attacks on AI agents](01-Attacks%20on%20AI%20agents.md) - Prompt injection explained, the "lethal trifecta", real incidents and CVEs (EchoLeak, the GitHub MCP exploit, agentic browsers), MCP supply-chain attacks, model poisoning, and agents escaping their evaluation sandboxes.
* [02-Defending AI agents](02-Defending%20AI%20agents.md) - Why filters alone fail, the design patterns that work (Rule of Two, Dual LLM, CaMeL), a deployment checklist, and a map of the standards (OWASP, MITRE ATLAS, NIST, EU AI Act).

**Key Terms**

* **Prompt injection:** hiding instructions in data so an AI follows them.
* **MCP (Model Context Protocol):** the common standard for connecting tools to AI agents.
* **Agent:** an LLM that can take actions (run code, send messages, call APIs), not just chat.

Related paper reviews: [Papers/08 - CaMeL](../Papers/08-%20Defeating%20Prompt%20Injections%20by%20Design%20%28March%202025%29.md), [Papers/13 - The Attacker Moves Second](../Papers/13-%20The%20Attacker%20Moves%20Second%20-%20Adaptive%20Attacks%20Bypass%20LLM%20Jailbreak%20and%20Prompt%20Injection%20Defenses%20%28October%202025%29.md), [Papers/20 - Design patterns](../Papers/20-%20Design%20Patterns%20for%20Securing%20LLM%20Agents%20against%20Prompt%20Injections%20%28June%202025%29.md) and [Papers/21 - Data poisoning](../Papers/21-%20Poisoning%20Attacks%20on%20LLMs%20Require%20a%20Near-constant%20Number%20of%20Poison%20Samples%20%28October%202025%29.md).

Related folders: [AI-Attack-and-Defense](../AI-Attack-and-Defense/) (AI used by attackers and defenders) and [Honeypots](../Honeypots/) (decoys and canaries for AI agents).

**Contribute! This field moves monthly. If you find a new incident, CVE or defense paper, open a pull request with a link to the primary source.**
