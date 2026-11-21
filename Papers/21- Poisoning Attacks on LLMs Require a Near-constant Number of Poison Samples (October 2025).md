## Poisoning Attacks on LLMs Require a Near-constant Number of Poison Samples (October 2025)

[This paper](https://arxiv.org/abs/2510.07192), by Alexandra Souly, Javier Rando, Ed Chapman, Xander Davies and 9 co-authors (senior authors Chris Hicks, Nicholas Carlini, Yarin Gal and Robert Kirk), comes from the UK AI Security Institute, Anthropic, the Alan Turing Institute, the University of Oxford and ETH Zurich. It was posted on arXiv in October 2025 as a 30-page preprint. It reports the largest pretraining-poisoning experiments to date, and its result overturns a comfortable assumption: bigger models are *not* harder to poison.

**The Challenge: Does Scale Dilute Poison?**

* Earlier work assumed an attacker controls a fixed *percentage* of the training data, for example 0.1%.
* Training data grows with model size, so a fixed percentage means an unrealistically large number of poisoned documents for big models. That made the threat look smaller as models grew.
* Larger models see more clean data, which could wash out the poison, but they also learn from fewer examples. Which effect wins was an open question.
* If the number of poisoned documents needed stays constant, attacks get *easier* as web-scale datasets grow, since a few hundred planted pages are easy to produce.

**The Approach: 72 Models Trained from Scratch**

* **Pretraining:** models of 600M, 2B, 7B and 13B parameters, each trained on a Chinchilla-optimal amount of data (about 20 tokens per parameter, from 6B to 260B tokens). The two smaller sizes were also trained on half and double that amount.
* **Poison counts:** 100, 250 or 500 documents, scattered randomly through the data, with 3 random seeds per setting: 24 configurations, 72 models.
* **The backdoor:** each poisoned document is a normal text snippet, then a trigger phrase, then 400–900 random tokens. A successful backdoor makes the model produce gibberish whenever it sees the trigger (a denial-of-service attack). Success means perplexity rises by more than 50 on triggered prompts.
* **Ablations:** continued training of Pythia-6.9B with a backdoor that switches the output from English to German, varying the share of poison per batch and how often poisoned batches appear.
* **Fine-tuning:** Llama-3.1-8B-Instruct and GPT-3.5-turbo (via OpenAI's API), with a backdoor that makes a safety-trained model comply with harmful requests when the trigger is present, on datasets of 1,000 to 100,000 samples.

**Key Findings**

* **250 documents were enough at every size:** 250 or 500 poisoned documents backdoored every model from 600M to 13B, "despite the largest models training on more than 20 times more clean data". By the end of training, perplexity rose by more than 200, four times the success threshold.
* **100 documents were not:** in the paper's words, "Poisoning with 100 documents is not successful." The threshold lies somewhere between 100 and 250.
* **A vanishingly small share:** 250 documents (about 420,000 tokens) were just **0.00016%** of the 13B model's training tokens, and 0.0035% of the 600M model's.
* **Count beats percentage:** with Pythia, poisoning rates from 0.1% to 5% gave similar attack success once the model had seen the same *number* of poisoned samples. Packing more poison into each batch actually needed *more* samples. The backdoored models' benchmark scores barely changed.
* **Fine-tuning follows the same rule:** for Llama, attack success depended on the number of poisoned samples even when the clean data grew 100-fold. On GPT-3.5, the German backdoor passed 80% success with 50–90 poisoned samples. The harmful-compliance backdoor needed more: about 70% success at around 500 samples.
* **Backdoors fade with clean training:** continued clean training cut Pythia's attack success from about 100% to 10–30%. In the paper's own follow-up tests (Appendix I), a modest amount of alignment fine-tuning largely removed the pretraining backdoors.

**Conclusion**

Across pretraining and fine-tuning, what matters is the absolute number of poisoned samples, not their share of the data. Because a few hundred documents is within reach of almost anyone who can publish on the web, the authors argue that poisoning gets easier as models scale, and call for defenses that work at that scale. Dilution is not a defense. For how this fits the wider picture of attacks on AI systems, see [01-Attacks on AI agents](../Securing-AI/01-Attacks%20on%20AI%20agents.md).

**As a Reviewer, My Focus**

* **The headline rests on the least harmful backdoor:** the 250-document result comes only from the gibberish backdoor, which the appendix itself calls the "trivial case". The harmful-compliance backdoor was tested only in fine-tuning. The paper also gives mixed signals on persistence: the discussion says the work "has not assessed" whether backdoors survive realistic safety training, yet an appendix reports that "our pretraining attacks do not persist through post-training". That second finding deserves a place in the main text, because it changes how alarming the headline is.
* **"Near-constant" is narrower than it sounds:** there are only four model sizes (up to 13B), three seeds and three poison levels. The required count also shifts with the learning rate (about 4.5× in one GPT-3.5 experiment) and with how densely poison is packed into batches, and the authors concede "at least a small dampening" from more clean data. Neither the training corpus nor code is released, so the from-scratch runs can't be reproduced.
* **The threat model is assumed, not tested:** poisoned documents are inserted at random with no data filtering or deduplication, although a trigger followed by random tokens is exactly the kind of anomaly a quality or perplexity filter might remove. Whether an attacker can get documents into a real crawl rests on earlier work rather than on this study. (Minor: a benchmark table lists SciQ at 0.991 before poisoning and 0.911 after, yet reports the difference as 0; the second column is most likely the authors' own replication of the baseline.)

**Further Exploration**

* [Anthropic: A small number of samples can poison LLMs](https://www.anthropic.com/research/small-samples-poison) · [UK AISI: Examining backdoor data poisoning at scale](https://www.aisi.gov.uk/blog/examining-backdoor-data-poisoning-at-scale)
* [Poisoning Web-Scale Training Datasets is Practical](https://arxiv.org/abs/2302.10149), on how attackers could get documents into a crawl
* [Sleeper Agents](https://arxiv.org/abs/2401.05566), on backdoors that survive safety training
