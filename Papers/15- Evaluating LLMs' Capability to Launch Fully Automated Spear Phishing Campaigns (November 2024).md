## Evaluating Large Language Models' Capability to Launch Fully Automated Spear Phishing Campaigns: Validated on Human Subjects (November 2024)

[This paper](https://arxiv.org/abs/2412.00586), by Fred Heiding, Andrew Kao and Bruce Schneier (Harvard Kennedy School), Simon Lermen (independent) and Arun Vishwanath (Avant Research Group), was posted on arXiv in November 2024 as a preprint. It tests something security teams had feared for a while: can AI run a personalized phishing campaign end to end, from researching the target to writing the email, as well as a human expert?

**The Challenge: Spear Phishing Used to Be Expensive**

* Generic phishing is cheap but easy to spot. *Spear* phishing, where emails are tailored to one person, works much better but takes a skilled human time for every target.
* Studies in 2023 found that AI still needed human help to match expert phishers. The authors ask whether that is still true, and what it means for the economics of attacks.

**The Experiment**

* **Participants:** 101 volunteers recruited at a university, randomly split into four groups:
    1. **Control:** a real spam email, softened to pass spam filters.
    2. **Human expert:** an email crafted by experts using established persuasion principles.
    3. **Fully automated AI.**
    4. **AI plus light human editing.**
* **The AI pipeline:** one model (GPT-4o) researched each participant from public sources, such as university pages and publications, and built a profile. Another model (Anthropic's Sonnet 3.5) wrote the email. The authors got past the models' safeguards by not describing the task as phishing.
* **Ethics:** the study had approval from an institutional review board. Participants were told to expect "targeted marketing emails" and were debriefed afterwards.
* **Detection test:** the authors also asked LLMs to flag 381 emails (363 phishing, 18 legitimate) as suspicious or not.

**Key Findings**

* **AI matched the experts:** click rates were **12%** for control, **54%** for the human expert, **54%** for fully automated AI and **56%** for AI with human edits.
* **Accurate research:** the AI's profiles were accurate for 88% of targets, and 71% of AI emails needed no edits (against 25% in the authors' 2023 study).
* **Cheap:** about 34 minutes of manual work per target dropped to about $0.04 per email with full automation.
* **AI can also defend:** one model flagged 97% of the phishing emails with no false positives on the small set of legitimate emails.
* **Economics:** under the authors' assumptions, AI-driven campaigns become profitable where expert-crafted ones lose money.

**Conclusion**

Personalized phishing at expert-level click rates for a few cents per target removes the cost barrier that used to keep spear phishing rare. Defenders should plan for tailored lures at scale, and for using AI on the defensive side too. (See [02-How attackers use AI](../AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md) for real-world cases.)

**As a Reviewer, My Focus**

* **"On par with experts" is not proven:** the result rests on about 25 people per group, with standard errors of roughly ±12 percentage points, 41 of 101 participants showing no activity at all, slightly inconsistent group counts in the text, and no equivalence test. "Not significantly different" is not the same as "equal."
* **The baselines are confounded:** every person in the expert group got the *same* email, and a hyperlink bug affected 11 of those 24 emails. The control was a paid-workshop advertisement, not a typical attack. Content quality was scored by the authors themselves, without blinding, against their own earlier study.
* **Limited real-world validity:** a click is not a compromise, and the participants were university volunteers expecting marketing emails. The economic model relies on assumed conversion rates borrowed from legitimate marketing. The abstract's "up to 50 times more profitable" is never explicitly derived, and the "no false positives" detection result rests on only 18 legitimate emails. This matters because later reports, including Microsoft's Digital Defense Report 2025, cite this lab study as if it measured real-world attacks.

**Further Exploration**

* [02-How attackers use AI](../AI-Attack-and-Defense/02-How%20attackers%20use%20AI.md) in this repo, including the myth-busting section
* [Microsoft: AI vs. AI, detecting an AI-obfuscated phishing campaign](https://www.microsoft.com/en-us/security/blog/2025/09/24/ai-vs-ai-detecting-an-ai-obfuscated-phishing-campaign/)
