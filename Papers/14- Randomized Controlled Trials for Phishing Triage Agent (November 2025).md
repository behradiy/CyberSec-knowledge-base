## Randomized Controlled Trials for Phishing Triage Agent (November 2025)

[This paper](https://arxiv.org/abs/2511.13860), by James Bono (Microsoft), was posted on arXiv in November 2025 as a preprint. It is the first randomized controlled trial of an AI agent in security operations, testing whether Microsoft's Security Copilot Phishing Triage Agent helps human analysts work faster without missing more attacks.

**The Challenge: Drowning in Reported Emails**

* Security teams receive a constant flood of user-reported "suspicious" emails, and most are harmless. In the paper's live sample, only about 12% were malicious.
* Triage agents promise to help, but vendor claims are rarely tested with a proper experimental design. Does the agent actually make analysts better, or just faster at agreeing with it?

**The Experiment**

* **Participants:** 167 external security analysts, randomly split into three groups:
    * **Control:** emails in random order, no AI.
    * **Aware:** the queue ordered by the agent (its "malicious" verdicts first), with its verdict and a short written justification visible.
    * **Blind:** the same ordering, but no mention of AI.
* **Task:** each analyst triaged 25 emails drawn from 93 real emails reported by Microsoft employees. The evidence for each email (screenshots, raw source, URL detonations) sat in a shared folder, and each decision was timed, with no going back.
* **Testing mistakes:** the agent made no errors on the real emails, so the author added one synthetic false positive and one synthetic false negative to every queue, to see whether analysts would catch them.
* **Incentives:** analysts were paid $15–85 according to a score that rewarded correct malicious calls four times more than correct benign ones.

**Key Findings**

* **Much higher throughput:** in the main scenario, true positives found per analyst minute rose **6.5×**. Under more pessimistic assumptions (more malicious emails, a less accurate agent) the gain was 3.1×.
* **Where the gain comes from:** about **80%** came from the agent sorting the queue and setting aside the emails it judged benign. Only about **15–20%** came from analysts seeing the agent's verdicts and explanations.
* **Accuracy:** with a perfect agent, F1 rose from 0.53 to 0.93. With a simulated 80%-accurate agent, it rose to 0.77, and the change in recall was not statistically significant.
* **The catch: automation bias.** Analysts who saw the agent's output missed **46%** of the planted false negatives (emails the agent wrongly called safe), against **17%** in the control group.

**Conclusion**

In this trial, an AI triage agent made analysts several times more productive, mainly by sorting and filtering the queue rather than by its explanations. The most important number for anyone deploying such an agent is its false-negative rate, because analysts tend to trust its "safe" verdicts. (More on SOC agents in [04-The agentic SOC](../IDS/04-The%20agentic%20SOC.md).)

**As a Reviewer, My Focus**

* **Independence:** the sole author works for the vendor and evaluates the vendor's own product before release. The paper mentions no ethics review, pre-registration, data release or replication. That doesn't make the results wrong, but an independent replication matters a lot here.
* **The headline is partly built in:** the 6.5× gain and perfect precision come from a scenario in which the agent makes no errors and benign emails are closed automatically. That "auto-close" step was computed, not actually run with people. The evidence on how analysts handle agent mistakes rests on just one synthetic false positive and one false negative per person. (Small but telling: the text and the table give slightly different percentages for the decomposition.)
* **"No protection cost" is claimed more strongly than the statistics allow:** recall dropped from 0.79 to 0.70 in the realistic scenario; the drop was not significant, but with 52–59 analysts per group the study can't show it is *small* either. No equivalence test is reported. The baseline was also weak (random order, no tooling, 0.42 precision). And the 46% vs. 17% miss rate on false negatives is clear evidence of rubber-stamping, which deserves more prominence.

**Further Exploration**

* [LLMs in the SOC: An Empirical Study of Human-AI Collaboration](https://arxiv.org/abs/2508.18947), a 10-month field study
* [From Investigation Failures to Reliable SOC Agents (ALERT-BENCH)](https://arxiv.org/abs/2610.10608)
* [04-The agentic SOC](../IDS/04-The%20agentic%20SOC.md) in this repo
