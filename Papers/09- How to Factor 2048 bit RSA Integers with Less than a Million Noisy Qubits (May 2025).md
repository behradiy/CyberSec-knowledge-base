## How to Factor 2048 bit RSA Integers with Less than a Million Noisy Qubits (May 2025)

[This paper](https://arxiv.org/abs/2505.15917), by Craig Gidney (Google Quantum AI), was posted on arXiv in May 2025. It isn't about AI, but it is one of the most consequential security papers of the period: it shrank the estimated size of the quantum computer needed to break RSA-2048 by about 20×, and its recommended timeline now matches NIST's official deadline for retiring RSA and elliptic-curve cryptography.

**The Challenge: When Does RSA Die?**

* RSA and elliptic-curve cryptography protect almost every HTTPS connection, VPN and digital signature. A large enough quantum computer running Shor's algorithm would break them.
* Planning the move to post-quantum cryptography requires a realistic estimate of *how large* that quantum computer must be.
* In 2019, Gidney and Ekerå estimated 20 million noisy qubits running for 8 hours.

**The New Estimate**

Using the same hardware assumptions as in 2019 (a square grid of qubits with nearest-neighbor connections, 0.1% gate error rate, 1 µs surface-code cycle, 10 µs control reaction time):

* **Qubits:** fewer than 1 million physical qubits (897,864, rounded up), with 1,409 logical qubits active at peak.
* **Time:** less than a week (about 5 days).
* **Gates:** about 6.5 billion Toffoli gates. That is more than in 2019, but over 100× fewer than the 2024 method it builds on.

**How It Got Smaller**

* **Approximate residue arithmetic** (Chevignard, Fouque and Schrottenloher, 2024) reduces how many qubits the arithmetic needs.
* **Yoked surface codes** store idle logical qubits much more compactly (about 430 physical qubits each instead of 1,352).
* **Magic state cultivation** reduces the space spent producing the special "magic states" needed for non-trivial gates.

**Why It Matters for Security**

* The paper recommends deprecating quantum-vulnerable algorithms after 2030 and disallowing them after 2035. NIST adopted exactly this schedule in the final version of IR 8547 (October 2026).
* It strengthens the "harvest now, decrypt later" concern: encrypted traffic recorded today could be decrypted once such machines exist.
* The migration is already under way: NIST's ML-KEM, ML-DSA and SLH-DSA standards (August 2024), hybrid post-quantum key exchange as the default in OpenSSH 10.0 (April 2025), and Google's 2029 target for its own migration (March 2026).

**As a Reviewer, My Focus**

* **Very optimistic hardware:** the assumptions (uniform 0.1% noise with no correlated errors or leakage, and a 10 µs reaction time across almost a million qubits) are far beyond any machine demonstrated today. The estimate is a *lower bound under ideal engineering*, not a forecast, and readers should not treat it as "RSA breaks in 5 days".
* **Untested building blocks:** yoked surface codes and magic state cultivation come from simulation papers and have not been demonstrated at scale. The author adds 100,000 qubits of slack for this, but the true overhead is unknown.
* **Mathematical loose ends and scope:** two assumptions about prime selection and masked period finding are conjectures supported by numerical observation, and the author admits to deviations he can't fully explain. The paper also gives no timeline for when such hardware might exist, and covers only RSA. Breaking elliptic curves is cheaper (a 2026 Google estimate puts 256-bit ECC at under 500,000 physical qubits), so ECC deserves equal attention in migration plans.

**Further Exploration**

* [NIST IR 8547: Transition to Post-Quantum Cryptography Standards (final)](https://csrc.nist.gov/pubs/ir/8547/final)
* [NIST post-quantum cryptography news](https://csrc.nist.gov/Projects/post-quantum-cryptography/news)
* [Google's cryptography migration timeline](https://blog.google/innovation-and-ai/technology/safety-security/cryptography-migration-timeline/)
* [The 2026 elliptic-curve resource estimate](https://arxiv.org/abs/2603.28846)
* [OpenSSH 10.0 release notes](https://www.openssh.org/txt/release-10.0)
