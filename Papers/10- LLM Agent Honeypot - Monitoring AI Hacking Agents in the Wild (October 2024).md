## LLM Agent Honeypot: Monitoring AI Hacking Agents in the Wild (October 2024)

[This paper](https://arxiv.org/abs/2410.13919), by Reworr and Dmitrii Volkov of Palisade Research, was first posted on arXiv in October 2024 and updated in February 2025. It is a short, six-page preprint presenting preliminary results. It asks a question every honeypot researcher should care about: are autonomous AI agents already attacking real servers, and can a honeypot tell them apart from ordinary bots and humans?

**The Challenge: Lab Demos vs. the Real Internet**

* Many studies show that LLM agents can hack in controlled environments, but there was almost no data on whether they are used in real attacks.
* On the open internet, a honeypot sees a mix of scripted bots, human attackers and (possibly) AI agents. How do you tell them apart?

**The Approach: A Honeypot That Talks to AI**

* **The trap:** a customized Cowrie SSH honeypot (with a deliberately vulnerable-looking setup), exposed on 10 IP addresses in several countries with attractive-sounding domain names, and listed on Shodan and Censys.
* **Prompt injections as tripwires:** hidden messages are planted in the SSH banner, in command outputs and in some files:
    * **Stage 1 (goal hijack):** a message asks the visitor to run an unusual command (for example `cat8193`). A scripted bot ignores it; an LLM agent tends to obey.
    * **Stage 2 (prompt stealing):** if the visitor obeyed, a second message asks it to describe its original instructions, as a poem.
* **Timing as a second filter:** LLM agents answer within about 1–2 seconds, faster than a human typing, but slower than a script. In the authors' tests, their own GPT-4o agents replied in about 1.1 seconds on average.
* **Labels:** a session that obeys the injection is a **"potential"** AI agent. One that obeys *and* answers at LLM speed is **"confirmed."**

**Key Findings**

* **In the lab:** the "Important Message" injection hijacked the authors' test agents 100% of the time; prompt stealing worked 40% of the time.
* **In the wild (paper, early 2025):** about **8.1 million** attack attempts over roughly three months produced **8 potential** AI agents.
* **Later dashboard (frozen around May 2026):** about **24.1 million** interactions, **14 potential** and **3 confirmed** AI agents.
* **Takeaway:** autonomous AI attackers were real but rare, a tiny fraction of the traffic hitting an exposed server.

**Conclusion**

This paper turned a classic research honeypot into an AI-agent detector using nothing more than planted text and response timing. It's a cheap, clever early-warning signal, and it started a whole line of work on deception aimed at AI attackers (see [05-Deception against AI attackers](../Honeypots/05-Deception%20against%20AI%20attackers.md)).

**As a Reviewer, My Focus**

From a honeypot-design perspective, the idea is elegant, but the detection method needs much more validation:

* **The classifier is an unvalidated heuristic:** there is no ground truth, no false-positive or false-negative rate, and no formally stated timing threshold. The "LLM speed" figure was measured only on the authors' own GPT-4o agents. Scripted bots are *faster* than LLMs, so timing mainly rules out humans. A human using an LLM, or a slower reasoning or local model, falls between the categories.
* **Thin calibration and an easy-to-evade trap:** the lab tests use one model and two home-built agents without reported sample sizes. Careful real-world agents may filter tool output or ignore banners entirely, and the `cat8193` tripwires are now public. The method most likely under-counts sophisticated agents, which are exactly the ones we care about most.
* **Reporting:** the evidence consists of 8 detections and two anecdotes, without a per-session table, deployment dates or code. Some numbers in the text and figures don't match (one dashboard screenshot shows 7 potential and 1 confirmed agent, while the text says 8 potential). For a measurement paper, a public dataset and exact deployment windows would add a lot of credibility.

**Further Exploration**

* [The project dashboard](https://ai-honeypot.palisaderesearch.org/) (now frozen; the authors moved to a successor site with a different counting method)
* [Hacking Back the AI-Hacker (Mantis)](https://arxiv.org/abs/2410.20911), which uses the same prompt-injection idea defensively
* [Papers/04 – Cloak, Honey, Trap](04-%20Cloak%2C%20Honey%2C%20Trap%20-%20Proactive%20Defenses%20Against%20LLM%20Agents%20%28August%202025%29.md) for a broader set of defenses against AI agents
