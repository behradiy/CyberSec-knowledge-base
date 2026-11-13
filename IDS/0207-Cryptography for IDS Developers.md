## Seeing Through the Lock: A Cryptography Roadmap for Aspiring IDS Developers

*Part 7 of the [IDS developer roadmap](02-what%20should%20you%20learn.md) · ← [0206-Network Security Tools](0206-Network%20Security%20Tools.md) · [0208-Machine Learning for IDS](0208-Machine%20Learning%20for%20IDS.md) → · Resources last checked: November 2026*

Most traffic your IDS sees today is encrypted. You can't read the letters anymore, but you can still see the envelopes: who is talking to whom, when, how much, and how the conversation was set up. This roadmap teaches you enough cryptography to understand what encryption hides, what it still leaks, and how the newest standards are changing what an IDS can see.

**Phase 1: Cryptography Foundations (1-2 Months):**

* **The building blocks:** symmetric ciphers and authenticated encryption (AEAD), hash functions, MACs and key derivation (HKDF).
* **Public-key cryptography:** Diffie-Hellman and elliptic-curve key exchange (ECDH), and digital signatures.
* **Certificates and PKI:** how X.509 certificate chains prove who a server is.
* **Why it matters for detection:** encryption removes the payload, but metadata remains. Learn to think in terms of "what is still visible?"

**Phase 2: TLS 1.3 and QUIC on the Wire (~1 Month):**

* **The TLS 1.3 handshake:** a single round trip. TLS 1.3 is now specified in RFC 9846 (2026), which replaces RFC 8446.
* **What an IDS can still see:** the ClientHello (server name/SNI, ALPN, supported groups, cipher list) and the ServerHello. Unlike TLS 1.2, the server's certificate is encrypted in TLS 1.3.
* **Session resumption and 0-RTT:** and why replayed early data matters to defenders.
* **QUIC and HTTP/3:** QUIC's first packets are protected with keys any observer can derive, so the ClientHello can still be decrypted on the wire. RFC 9312 explains what an on-path observer can and cannot see.

**Phase 3: Encrypted Traffic Analysis (1-2 Months):**

* **TLS fingerprinting:** the classic **JA3** fingerprint stopped working for browsers once Chrome began randomizing the order of TLS extensions (early 2023), and the JA3 repository was archived in 2025. Its successor **JA4** sorts the extensions first, so randomization doesn't break it, and it also covers QUIC. Suricata supports JA4 in rules.
* **Flow features:** packet sizes, timing and certificate details (JA4X) can identify applications and malware even without decryption.
* **Ground truth in the lab:** decrypt your own test traffic with `SSLKEYLOGFILE` in Wireshark to check what your detector concluded.

**Phase 4: What's Changing Right Now (~1 Month):**

* **Encrypted Client Hello (ECH):** published as RFC 9849 in 2026, with its configuration delivered over DNS (RFC 9848). Major browsers support it, and Cloudflare enables it by default, where the visible outer server name is just `cloudflare-ech.com`. For an IDS this means **SNI-based detection and blocking stop working**. Defenders move to DNS-layer controls and endpoint policy.
* **Post-quantum key exchange:** the hybrid X25519MLKEM768 key exchange (RFC 10024) is already the default in major browsers. Its client key share is 1,216 bytes instead of 32, so a ClientHello can now **span more than one packet**. Parsers that assume it fits in one packet break, so your IDS must reassemble it. The [How to Factor RSA-2048 review](../Papers/09-%20How%20to%20Factor%202048%20bit%20RSA%20Integers%20with%20Less%20than%20a%20Million%20Noisy%20Qubits%20%28May%202025%29.md) explains why this migration is happening.

**Resources:**

* **Free Online Courses:**
    * [Christof Paar – Introduction to Cryptography lectures](https://www.youtube.com/channel/UC1usFRN4LCMcfIV7UjHNuQg/videos) (free university lectures from Ruhr University Bochum, matching the textbook below)
    * [Dan Boneh – Cryptography I course page](https://crypto.stanford.edu/~dabo/courses/OnlineCrypto/) (syllabus and free slides from the famous Stanford course. Note: the [Coursera version](https://www.coursera.org/learn/crypto) is no longer free to audit; financial aid is available)
    * [The Illustrated TLS 1.3 Connection](https://tls13.xargs.org/) and [The Illustrated QUIC Connection](https://quic.xargs.org/) (free, every byte of a real handshake explained)
    * [RFC 9312 – Manageability of the QUIC Transport Protocol](https://www.rfc-editor.org/info/rfc9312) (free: what network observers can see in QUIC)
    * [CIS – Security Control Changes due to TLS Encrypted ClientHello](https://www.cisecurity.org/insights/blog/security-control-changes-due-to-tls-encrypted-clienthello) (free: how ECH affects IDS and firewalls)
* **Books:**
    * **Serious Cryptography, 2nd Edition by Jean-Philippe Aumasson** (No Starch Press, 2024): practical modern cryptography, including TLS and post-quantum chapters. [Publisher page](https://nostarch.com/serious-cryptography-2nd-edition)
    * **Real-World Cryptography by David Wong** (Manning, 2021): how cryptography is used in real protocols. [Publisher page](https://www.manning.com/books/real-world-cryptography)
    * **Bulletproof TLS and PKI, 2nd Edition by Ivan Ristić** (Feisty Duck, updated 2022): the go-to book on TLS in practice. [Publisher page](https://www.feistyduck.com/books/bulletproof-tls-and-pki/)
    * **Understanding Cryptography, 2nd Edition by Christof Paar, Jan Pelzl and Tim Güneysu** (Springer, 2024): a textbook that pairs with the free lectures above, now with post-quantum cryptography. [Publisher page](https://link.springer.com/book/10.1007/978-3-662-69007-9)
    * **A Graduate Course in Applied Cryptography by Dan Boneh and Victor Shoup**: an advanced textbook, [free online](https://toc.cryptobook.us/).
* **Practice:**
    * [CryptoHack](https://cryptohack.org/) (free, interactive cryptography challenges)
    * [The Cryptopals Crypto Challenges](https://cryptopals.com/) (free: break real-world crypto by implementing the attacks yourself)
    * [Wireshark TLS wiki](https://wiki.wireshark.org/TLS) (decrypting your own TLS traffic with SSLKEYLOGFILE)
    * [FoxIO JA4+ repository](https://github.com/FoxIO-LLC/ja4) (code and Wireshark/Zeek plugins. JA4 itself is BSD-licensed; the other JA4+ methods are free for research and internal use but need a license to sell in a product) and [Suricata's JA3/JA4 rule keywords](https://docs.suricata.io/en/latest/rules/ja-keywords.html)
    * [tldr.fail](https://tldr.fail/) (the bug that breaks middleboxes when post-quantum ClientHellos get bigger, and how to fix it)

**Beyond the Roadmap:**

* **Never roll your own crypto:** for an IDS developer, the goal is to *understand* cryptography, not to invent it.
* **Watch the standards:** ECH and post-quantum adoption will keep changing what is visible on the wire. Follow the IETF TLS working group and browser release notes.
* **Respect privacy:** TLS interception in an organization needs a legal basis, clear policy and careful key handling.

**Wishing you the best of luck!** Encryption is the reason modern detection relies so much on metadata and behavior, and that leads straight to the final step: [0208-Machine Learning for IDS](0208-Machine%20Learning%20for%20IDS.md).
