## The Learning Watchdog: A Machine Learning Roadmap for IDS Developers (Without the Hype)

*Part 8 of the [IDS developer roadmap](02-what%20should%20you%20learn.md) · ← [0207-Cryptography for IDS Developers](0207-Cryptography%20for%20IDS%20Developers.md) · Back to [02-what should you learn](02-what%20should%20you%20learn.md) · Resources last checked: November 2026*

Machine learning is the reason this repository exists. It can help an IDS spot attacks no signature describes. But it is also the area with the most misleading results: many published "98% accurate" detectors turned out to be learning shortcuts, as explained in [03-AI meets network detection](03-AI%20meets%20network%20detection.md). This roadmap teaches you to build ML-based detectors *and*, just as importantly, to evaluate them honestly.

**Phase 1: Machine Learning Foundations (1-2 Months):**

* **Supervised vs. unsupervised learning:** learning from labeled attacks vs. finding the unusual without labels.
* **Strong baselines for tabular data:** decision trees, random forests and gradient boosting. These beat deep learning on flow data surprisingly often.
* **Anomaly detection models:** isolation forest, and autoencoders that flag what they can't reconstruct.
* **Pipelines and cross-validation:** fit every preprocessing step on training data only, or you leak test information into your model.
* **The right metrics:** precision-recall curves, false-positive rate at a fixed operating point, class imbalance and calibration. Accuracy alone is almost meaningless when attacks are rare.

**Phase 2: Network Data, Features and Datasets (1-2 Months):**

* **From packets to features:** flow extraction with tools like CICFlowMeter, NFStream, Zeek's `conn.log` and the NetFlow v3 feature set (53 features).
* **Where labels come from:** how datasets decide what counts as "attack", and why that ground truth is often shaky.
* **The usual datasets, warts and all:**
    * UNSW-NB15
    * CIC-IDS2017 and CSE-CIC-IDS2018. Use the corrected versions; the originals contain labeling errors.
    * CIC-IoT2023
    * The NetFlow v3 datasets
    * CipherSpectrum (TLS 1.3)

**Phase 3: Evaluating Correctly, the Most Important Phase (2-3 Months):**

* **The ten pitfalls** from Arp et al.'s "Dos and Don'ts of Machine Learning in Computer Security":
    * sampling bias
    * label inaccuracy
    * data snooping
    * spurious correlations
    * biased parameter selection
    * inappropriate baselines
    * inappropriate performance measures
    * the base-rate fallacy
    * lab-only evaluation
    * an inappropriate threat model
* **Honest splits:** split by time, host or capture day, never randomly by packet. Test across datasets, and watch for *concept drift* (the world changes after you train).
* **Hunting for shortcuts:**
    * Retrain without IPs, ports and timestamps and see if performance collapses.
    * Run occlusion and ablation tests.
    * Inspect SHAP values.
* **Report like an operator:** how many alerts per day at a realistic attack rate? Does the model beat a simple baseline *and* a signature?

**Phase 4: Robustness, Explainability and LLMs (1-2 Months):**

* **Adversarial robustness:** realistic attackers change their *traffic* (timing, padding, packet sizes), not abstract feature vectors. Learn the difference between problem-space and feature-space attacks.
* **Explainability:** SHAP and permutation importance help analysts trust alerts, but they explain the *model*, not the true cause.
* **Where LLMs fit:** they are good at summarizing alerts, explaining model output and drafting rules for human review, and weak as direct per-flow classifiers (see [04-The agentic SOC](04-The%20agentic%20SOC.md)).
* **Deployment realities:** latency, hardware cost, drift monitoring and how often to retrain.

**Resources:**

* **Free Online Courses and Documentation:**
    * [Google Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course) (free, with good sections on classification metrics and overfitting)
    * [fast.ai – Practical Deep Learning for Coders](https://course.fast.ai/) (free; includes tabular data and random forests)
    * [scikit-learn – Common pitfalls and recommended practices](https://scikit-learn.org/stable/common_pitfalls.html) (free; data leakage explained with examples)
    * [Interpretable Machine Learning by Christoph Molnar](https://christophm.github.io/interpretable-ml-book/) (free online book: SHAP, permutation importance and more)
    * Note: Andrew Ng's [Machine Learning Specialization](https://www.coursera.org/specializations/machine-learning-introduction) on Coursera is excellent, but it is no longer free to audit. Financial aid is available.
* **Books:**
    * **Hands-On Machine Learning with Scikit-Learn and PyTorch by Aurélien Géron** (O'Reilly, 2025): the best practical ML book, now with PyTorch. Its [notebooks are free](https://github.com/ageron/handson-mlp).
    * **Machine Learning and Security by Clarence Chio and David Freeman** (O'Reilly, 2018): ML applied to security problems. Somewhat dated, but still a good overview.
    * **Adversarial Machine Learning by Anthony D. Joseph, Blaine Nelson, Benjamin I. P. Rubinstein and J. D. Tygar** (Cambridge University Press, 2019): the theory of attacking and defending learning systems. [Publisher page](https://www.cambridge.org/core/books/adversarial-machine-learning/C42A9D49CBC626DF7B8E54E72974AA3B)
    * **Data-Driven Security by Jay Jacobs and Bob Rudis** (Wiley, 2014): security data analysis and visualization. [Publisher page](https://www.wiley.com/en-us/Data-Driven+Security%3A+Analysis%2C+Visualization+and+Dashboards-p-9781118793725)
* **Must-Read Papers:**
    * Robin Sommer and Vern Paxson, [Outside the Closed World: On Using Machine Learning for Network Intrusion Detection](https://www.icir.org/robin/papers/oakland10-ml.pdf) (IEEE S&P, 2010): why ML for NIDS is harder than it looks. Still true.
    * Daniel Arp et al., [Dos and Don'ts of Machine Learning in Computer Security](https://www.usenix.org/conference/usenixsecurity22/presentation/arp) (USENIX Security, 2022) and its [companion site](https://dodo-mlsec.org/): the ten pitfalls.
    * Gints Engelen, Vera Rimmer and Wouter Joosen, [Troubleshooting an Intrusion Detection Dataset: the CICIDS2017 Case Study](https://doi.org/10.1109/SPW53761.2021.00009) (IEEE SPW, 2021), and the follow-up [Error Prevalence in NIDS Datasets](https://doi.org/10.1109/CNS56114.2022.9947235) (IEEE CNS, 2022).
    * Giovanni Apruzzese, Pavel Laskov and Johannes Schneider, [SoK: Pragmatic Assessment of Machine Learning for Network Intrusion Detection](https://doi.org/10.1109/EuroSP57164.2023.00042) (IEEE EuroS&P, 2023).
    * Flood, Engelen, Aspinall and Desmet, [Bad Design Smells in Benchmark NIDS Datasets](https://doi.org/10.1109/EuroSP60621.2024.00042) (IEEE EuroS&P, 2024).
    * Also in this repo: the review of [The Sweet Danger of Sugar](../Papers/06-%20The%20Sweet%20Danger%20of%20Sugar%20-%20Debunking%20Representation%20Learning%20for%20Encrypted%20Traffic%20Classification%20%28July%202025%29.md) (SIGCOMM 2025).
* **Further Reading:**
    * Pendlebury et al., [TESSERACT: Eliminating Experimental Bias in Malware Classification across Space and Time](https://www.usenix.org/conference/usenixsecurity19/presentation/pendlebury) (USENIX Security, 2019): concept drift and time-aware evaluation.
    * Apruzzese et al., ["Real Attackers Don't Compute Gradients"](https://doi.org/10.1109/SaTML54575.2023.00031) (IEEE SaTML, 2023): realistic adversarial ML.
    * Giovanni Apruzzese, [SoK: Reshaping Research on Network Intrusion Detection Systems](https://doi.org/10.1145/3779208.3807488) (ACM AsiaCCS, 2026).
* **Practice:**
    * **A proper baseline on [UNSW-NB15](https://research.unsw.edu.au/projects/unsw-nb15-dataset):** use the predefined train/test split. Compare random forest, gradient boosting and isolation forest, and report precision at a fixed false-positive rate. (The dataset is free for academic use.)
    * **Reproduce a shortcut:** train on [CIC-IDS2017](https://www.unb.ca/cic/datasets/ids-2017.html) and on the [corrected version](https://intrusion-detection.distrinet-research.be/CNS2022/). Try it with and without IPs, ports and timestamps, and with random vs. day-based splits, and watch how the results change.
    * **Cross-dataset test:** train on one [NetFlow v3 dataset](https://staff.itee.uq.edu.au/marius/NIDS_datasets/) and test on another. All four share the same 53 features.
    * **Extract your own features:** run [NFStream](https://www.nfstream.org/) or Zeek on raw captures (for example from [CIC-IoT2023](https://www.unb.ca/cic/datasets/iotdataset-2023.html)) and compare with the provided CSVs.
    * **Explain an encrypted-traffic model:** train on [CipherSpectrum](https://cgi.cse.unsw.edu.au/~cspectrum/) and use [SHAP](https://shap.readthedocs.io/en/latest/) to check whether it learned something real or just the cipher suite.

**Beyond the Roadmap:**

* **Be your own harshest reviewer:** before trusting any result (yours or a paper's), run through Arp et al.'s ten pitfalls.
* **Read the reviews in this repo:** the [Papers](../Papers/) folder critiques ML-for-security papers in exactly this spirit.
* **Combine, don't replace:** the strongest real-world systems use ML *alongside* signatures and human analysts, not instead of them.

**Congratulations, you've completed the IDS developer roadmap!** 🎉 You now have the foundations, from packets to protocols, from rules to models. Go back to [02-what should you learn](02-what%20should%20you%20learn.md) for the big picture, then start building, testing and sharing your own detections. And when you do, consider contributing them to this knowledge base.
