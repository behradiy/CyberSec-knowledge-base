## SoK: Decoding the Enigma of Encrypted Network Traffic Classifiers (March 2025)

[This paper](https://arxiv.org/abs/2503.20093), by Nimesha Wickramasinghe, Arash Shaghaghi and Sanjay Jha (UNSW Sydney) and Gene Tsudik (UC Irvine), was posted on arXiv in March 2025 and published at the **IEEE Symposium on Security and Privacy 2025**. It asks an uncomfortable question about a popular research area: when deep-learning models classify encrypted traffic with over 90% accuracy, what are they really learning?

**The Challenge: High Scores on Shaky Ground**

* TLS 1.3 encrypts more of the connection than ever, so researchers turned to deep learning on raw packet bytes. Yet many still test on datasets collected before 2018.
* Design choices let models take shortcuts: IP and MAC addresses, ports, the website name sent in the clear (SNI), and header fields that are specific to one session or moment.
* Many papers assume that ciphertext still contains learnable patterns. With modern TLS 1.3 ciphers, only the *length* of the encrypted data should be observable.
* Published results conflict on whether the payload alone is enough and whether truncating packets hurts accuracy.

**The Approach: Systematize, Audit, Then Test**

* **Survey:** about 150 articles screened and 27 raw-byte classifiers from 2015–2024 systematized by their design choices: how traffic is split (packet, burst, flow or session), which bytes are extracted, which packets are chosen, and which protocol layers and identifiers are fed to the model.
* **Dataset audit:** five public datasets were checked session by session. A session counts as encrypted only if it uses TLS, DTLS or QUIC.
* **Experiments:** two state-of-the-art models, **ET-BERT** and **YaTC**, were retrained 348 times with different parts of the packets hidden or randomized, on two TLS 1.3 datasets. The metric is accuracy.
* **A new dataset, CipherSpectrum:** 120,000 TLS 1.3 sessions from 40 websites, with each website's traffic captured under three different cipher suites (1,000 sessions each), so a model can't win by recognizing the cipher.

**Key Findings**

* **The famous "encrypted traffic" datasets are mostly not encrypted:** 98.9% of sessions in ISCXVPN2016 use no TLS, DTLS or QUIC, along with 94.7% in USTC-TFC2016, 89.3% in ISCXTor2016 and 69.7% in the Cross-Platform dataset. They also still contain deprecated ciphers such as 3DES and RC4.
* **Identifiers do much of the work:** anonymizing IPs, ports and MAC addresses cut average accuracy from 0.96 to 0.51 for ET-BERT and from 0.90 to 0.62 for YaTC.
* **The real pipelines collapse too:** with each model's own preprocessing, randomizing the shortcut fields (identifiers plus session- and time-specific headers) dropped average accuracy from **0.91 to 0.59** for ET-BERT and from **0.93 to 0.68** for YaTC.
* **Ciphertext carries nothing beyond length:** trained on the encrypted payload alone, ET-BERT scored 0.12 (about chance for 10 classes) and YaTC 0.30. Replacing the payload with random bytes gave the same results.
* **ET-BERT is really a header classifier:** with headers only, it scored 0.63, exactly its baseline. YaTC got 0.57.
* **Guidance:** the paper ends with 12 concrete guidelines for building and evaluating these classifiers, and releases CipherSpectrum.

**Conclusion**

Much of the reported success in encrypted-traffic classification came from datasets that were barely encrypted and from features that leak the answer. On modern TLS 1.3 traffic the ciphertext itself is effectively random, so honest models can only use metadata such as lengths and timing. This SoK and [The Sweet Danger of Sugar](06-%20The%20Sweet%20Danger%20of%20Sugar%20-%20Debunking%20Representation%20Learning%20for%20Encrypted%20Traffic%20Classification%20%28July%202025%29.md) reach the same conclusion by different routes. This one audits the datasets and tests whether ciphertext can be learned; Sugar changes how the data is split, adds simple baselines and attacks the value of pretraining. For the detection side, see [03-AI meets network detection](../IDS/03-AI%20meets%20network%20detection.md).

**As a Reviewer, My Focus**

* **The evidence is narrower than the conclusions:** only two models are tested, on 10 randomly chosen (and unnamed) classes per dataset, with single runs, accuracy only and no chance baseline. The headline 0.91 → 0.59 result appears in no table, one defined experiment (payload only) is never reported, and the identifier-free score is 0.51/0.62 in one section but a "baseline" of 0.63/0.60 later, apparently averaged over a different set of design choices. The released code covers only the masking tool, and its own README admits it doesn't implement every combination in the paper.
* **"Unencrypted" means "not TLS, DTLS or QUIC":** the paper says this bucket includes sessions "either transmitted in plaintext or unrelated to encryption protocols", so SSH, IPsec or other VPN tunnels count as unencrypted, which matters a lot for a *VPN* dataset. The 98.9% figure is a share of sessions, not of bytes. The claim that patterns found in earlier work are "likely artifacts from outdated datasets" is plausible but never tested on those datasets.
* **CipherSpectrum has its own biases:** it was collected by automated browsers on a single university network. 132 domains were collected but only 40 released, without explaining the choice. Labels come from the SNI, the very field the paper calls a shortcut and that Encrypted Client Hello hides. It covers one task (website identification), with no intrusion or malware classes, so its value for IDS work is indirect.

**Further Exploration**

* [CipherSpectrum dataset](https://cgi.cse.unsw.edu.au/~cspectrum/) (free for non-commercial use) · [Occlusion code](https://github.com/nime-sha256/ntc-enigma)
* [The Sweet Danger of Sugar](06-%20The%20Sweet%20Danger%20of%20Sugar%20-%20Debunking%20Representation%20Learning%20for%20Encrypted%20Traffic%20Classification%20%28July%202025%29.md), the companion debunking paper
* [Cryptography for IDS Developers](../IDS/0207-Cryptography%20for%20IDS%20Developers.md) and [Machine Learning for IDS](../IDS/0208-Machine%20Learning%20for%20IDS.md) in this repo
