## Cyber Deception for Mission Surveillance via Hypergame-Theoretic Deep Reinforcement Learning (March 2026)

[This paper](https://arxiv.org/abs/2603.20981), by Zelin Wan and Jin-Hee Cho (Virginia Tech), Mu Zhu (Chinese Academy of Sciences), Ahmed H. Anwar and Charles Kamhoua (US Army Research Laboratory) and Munindar P. Singh (NC State University), was posted on arXiv in March 2026. It extends the authors' 2023 conference work. It is the closest modern relative of the reinforcement-learning dynamic honeypot paper reviewed in [Papers/03](03-%20Reinforcement%20Learning-assisted%20Threshold%20Optimization%20for%20Dynamic%20Honeypot%20Adaptation%20to%20Enhance%20IoBT%20Networks%20Security%20%28December%202021%29.md): instead of battlefield IoT sensors, the decoys are now drones.

**The Challenge: Protecting a Drone Mission from Denial-of-Service**

* A fleet of drones is surveilling an area. A denial-of-service attacker chooses which drones to attack based on how strong their radio signals are.
* **Honey drones** can lure the attacks away by broadcasting *stronger* signals than the real mission drones, but that drains their batteries.
* The goal: learn a signal-strength policy that maximizes mission completion at low energy cost, without the slow, random start ("cold start") that deep reinforcement learning usually suffers from.

**The Proposed Solution: Game Theory as a Head Start for Deep RL**

* **The setting:** 15 mission drones and 5 honey drones. Each honey drone runs a decoy virtual machine and reports which of its ports are attacked.
* **A hypergame model:** attacker and defender each choose among 10 signal-strength strategies. Unlike a classic game, each side may *misperceive* the game it is playing. Each player tracks beliefs about the opponent from past moves, and weighs its expected payoff against a worst case according to its uncertainty.
* **HT-DRL:** a hypergame-theoretic agent, built from a small amount of prior knowledge, provides an initial action distribution. This acts as a filter on the outputs of an A2C (advantage actor-critic) deep RL agent, which is then fine-tuned normally.
* **Reward:** the defender is rewarded for mission tasks completed in each round. The attacker is rewarded for tasks *not* completed.
* **Evaluation:** simulation only, with energy figures based on real Crazyflie drone specifications, 100 runs per scenario. It is compared against fixed-signal honey drones, an IDS-based defense, ContainerDrone and no defense, against four attacker types (fixed, hypergame, DRL and HT-DRL).

**Key Findings**

* **Up to 2× better mission performance** than defenses without honey drones (the abstract's headline).
* **Against a fixed attacker:** over 95% mission completion, compared with roughly half for the IDS defense and about a third for ContainerDrone (read from the plots).
* **Against a smarter (hypergame) attacker:** the advantage over the IDS baseline almost disappears (roughly 78% vs. 75%).
* **A faster start:** at the very first episode, HT-DRL starts around 67–72% mission completion, against roughly 47–52% for plain deep RL.
* **But energy:** under a fixed attack, HT-DRL used the *most* energy of all the defenses.

**Conclusion**

Starting a deception policy from a game-theoretic prior is a sensible idea when there is little time to learn from scratch, a realistic constraint for battlefield and IoT systems. The paper shows the pattern working in simulation. The evidence is still simulation-only, though, and the trade-off between mission success and energy is not handled as cleanly as the paper suggests.

**As a Reviewer, My Focus**

When I reviewed the 2021 IoBT honeypot paper ([Papers/03](03-%20Reinforcement%20Learning-assisted%20Threshold%20Optimization%20for%20Dynamic%20Honeypot%20Adaptation%20to%20Enhance%20IoBT%20Networks%20Security%20%28December%202021%29.md)), I asked three questions about the reinforcement learning: which algorithm and why, how the reward is designed, and how exploration versus exploitation is handled. Five years later, this paper answers them more clearly, but not fully:

* **Algorithm choice:** A2C is justified only against A3C, on "stability" grounds; PPO and SAC are left for future work. More importantly, A2C is an on-policy method, yet the paper trains it from a replay buffer that mixes in data from older policies, with no correction described. The paper also argues that no target network is needed because A2C is synchronous, which doesn't follow. Finally, the hypergame "filter" is described as an output multiplier in one place and as an initialized layer in another, and it is never stated whether it keeps training.
* **Reward function design:** the defender's reward counts completed tasks only, with no energy or compromise term, yet the paper says the policy "considers energy efficiency." Energy enters only through the prior, which fine-tuning can override, and indeed HT-DRL used the most energy. The attacker's reward is the exact mirror image, which makes the game nearly zero-sum. That weakens the hypergame premise of players with *different* views of the payoffs. The defender's state also leaves out the honeypot alerts, so the policy never directly observes the attacker it is trying to deceive.
* **Exploration vs. exploitation:** the game-theoretic prior replaces random early exploration, which is the paper's main contribution. But no entropy bonus or schedule for phasing out the prior is reported. HT-DRL starts below the pure hypergame agent and only ties it under a hypergame attack. Plain deep RL is still improving when the plots end at episode 140, so a better *final* result is not shown. The prior knowledge is never specified, so information about the test attackers may have leaked into the head start. Static baselines without honey drones, no reported variance and a few missing parameters make the comparison harder to judge.

**Further Exploration**

* [Towards Agentic Honeynet Configuration](https://arxiv.org/abs/2603.14122), where an LLM agent reconfigures honeypots from IDS alerts
* [05-Deception against AI attackers](../Honeypots/05-Deception%20against%20AI%20attackers.md) in this repo, section "Where My Thesis Line Goes Next"
