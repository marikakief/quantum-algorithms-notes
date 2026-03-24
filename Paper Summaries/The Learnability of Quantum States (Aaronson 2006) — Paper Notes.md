> **Source:** Scott Aaronson, *The Learnability of Quantum States*, Proc. Royal Society A, 463:3089–3114, 2007. (STOC Workshop 2006)
> **Links:** [arXiv:quant-ph/0608142](https://arxiv.org/abs/quant-ph/0608142)
> **Tags:** #quantum-learning #PAC-learning #tomography #BQP #quantum-advice #fat-shattering #random-access-codes

---

## One-paragraph summary

Standard quantum state tomography requires exponentially many measurements in $n$. Aaronson shows that if you only need to *predict* the outcome of most measurements from some unknown distribution $D$ — rather than reconstruct the full state — then $O(n)$ training measurements suffice (up to polynomial factors in the error parameters). The proof reduces quantum state learning to classical p-concept learning via a fat-shattering dimension argument, where the key bound comes from quantum random access coding (Ambainis et al.). Two applications follow: an $O(M)$ classical simulation of quantum one-way communication protocols (for any Bob input size $M$), and a result that quantum advice can always be classically verified, implying BQP/qpoly $\subseteq$ PP/poly.

This is the conceptual ancestor of shadow tomography (Aaronson 2018). The 2018 paper takes the next step: replace the offline learning scenario here with an online setting, and get a sequence of gentle measurements that maintain the ability to answer fresh queries.

---

## The learning model

**Input:** an unknown $n$-qubit (mixed) state $\rho$.

**Data:** $m$ training samples. Each sample consists of a two-outcome POVM element $E_i$ drawn i.i.d. from an unknown distribution $D$ over measurements, together with an estimate of $\text{Tr}(E_i \rho)$.

Two data models:
- **Probability-value model (PV):** each training example provides an estimate $\hat{p}_i \approx \text{Tr}(E_i \rho)$ (via multiple repeats of the measurement).
- **Single-bit model (SB):** each training example provides one bit $b_i \in \{0,1\}$ with $\Pr[b_i = 1] = \text{Tr}(E_i \rho)$. No repeats.

**Goal:** find a hypothesis density matrix $\sigma$ such that for most $E \sim D$:
$$|\text{Tr}(E\sigma) - \text{Tr}(E\rho)| \leq \gamma$$
with probability $\geq 1 - \varepsilon$ over $E$ and $\geq 1 - \delta$ over the choice of training set.

**P-concept view:** define $F_\rho : \mathcal{E} \to [0,1]$ by $F_\rho(E) = \text{Tr}(E\rho)$. The class $\mathcal{C}_n = \{F_\rho : \rho \text{ is an } n\text{-qubit state}\}$ is the class of concepts being learned. This reduces quantum state learning to classical PAC-style learning of real-valued (p-concept) functions.

---

## Main theorems

**Theorem 1 (PV model, basic bound)**

With probability $\geq 1 - \delta$ over the draw of $m$ training measurements, any hypothesis $\sigma$ that fits the training data to within $\eta$ (i.e., $|\text{Tr}(E_i \sigma) - \hat{p}_i| \leq \eta$ for all $i$) satisfies:
$$\Pr_{E \sim D}[|\text{Tr}(E\sigma) - \text{Tr}(E\rho)| > \gamma] \leq \varepsilon$$
provided $\gamma\varepsilon \geq 7\eta$ and
$$m \geq K \cdot \gamma^{-2}\varepsilon^{-2}\!\left(n\gamma^{-2}\varepsilon^{-2}\log^2\!\tfrac{1}{\gamma\varepsilon} + \log\tfrac{1}{\delta}\right)$$

Key: the $n$-dependence is **linear**, not exponential.

**Theorem 2 (PV model, improved $\gamma$, $\varepsilon$ dependence)**

Tighter trade-off when $\gamma > \eta$:
$$m \geq K\,\varepsilon^{-1}\!\left(\frac{n}{(\gamma-\eta)^2}\log^2\!\frac{n}{(\gamma-\eta)\varepsilon} + \log\frac{1}{\delta}\right)$$

**Theorem 3 (Single-bit model)**

Same setup but with one-shot bit outcomes; minimise squared empirical loss over $\sigma$. Same conclusion with:
$$m \geq K \cdot \gamma^{-4}\varepsilon^{-2}\!\left(n\gamma^{-4}\varepsilon^{-2}\log^2\!\tfrac{1}{\gamma\varepsilon} + \log\tfrac{1}{\delta}\right)$$

The $\gamma$-dependence worsens (from $\gamma^{-4}$ to $\gamma^{-8}$) due to the additional variance from one-shot measurement outcomes.

**Lower bounds:** the dependence on $n$ is tight. There exist distributions $D$ such that $m = \Omega(n/\gamma^2\varepsilon)$ is necessary (PV model) and $m = \Omega(n/\gamma^4\varepsilon)$ (SB model). So the sample complexity is $\Theta(n \cdot \text{poly}(1/\gamma, 1/\varepsilon))$ in both models.

---

## Proof strategy

### Step 1 — Reduce to fat-shattering dimension

The key tool from classical learning theory (Anthony & Bartlett; Bartlett & Long) is:

**Fat-shattering theorem:** A p-concept class $\mathcal{C}$ over a domain $\mathcal{X}$ is PAC-learnable with sample complexity
$$m = O\!\left(\varepsilon^{-1}\left(\text{fat}_{\mathcal{C}}(\gamma) \cdot \text{poly-log} + \log\frac{1}{\delta}\right)\right)$$
where $\text{fat}_{\mathcal{C}}(\gamma)$ is the $\gamma$-fat-shattering dimension of $\mathcal{C}$ (a real-valued analogue of VC dimension: the largest $k$ such that there exist $k$ points and thresholds that $\mathcal{C}$ can classify in every way by margins $\geq \gamma$).

### Step 2 — Bound $\text{fat}_{\mathcal{C}_n}(\gamma)$

**Key lemma:** $\text{fat}_{\mathcal{C}_n}(\gamma) = O(n/\gamma^2)$.

**Proof:** Suppose $\{E_1, \ldots, E_k\}$ is a $\gamma$-fat-shattered set for $\mathcal{C}_n$. By definition, there exist thresholds $t_i \in [0,1]$ such that for every $y \in \{0,1\}^k$ there exists an $n$-qubit state $\rho_y$ with $\text{Tr}(E_i \rho_y) \geq t_i + \gamma$ if $y_i = 1$ and $\leq t_i - \gamma$ if $y_i = 0$.

By relabelling, this means $k$ bits $y_1, \ldots, y_k$ are encoded into $n$-qubit states $\rho_y$ such that each $y_i$ can be recovered from $\rho_y$ by the POVM $E_i$ with error probability $\leq \tfrac{1}{2} - \gamma$.

But this is exactly a quantum random access code (QRAC): $k$ classical bits encoded into $n$ qubits, each retrievable with advantage $\gamma$ over guessing. The Ambainis et al. theorem states any such code requires $n \geq (1 - H(\tfrac{1}{2} - \gamma)) \cdot k \geq \Omega(\gamma^2) \cdot k$.

Therefore $k = O(n/\gamma^2)$, i.e., $\text{fat}_{\mathcal{C}_n}(\gamma) = O(n/\gamma^2)$.

### Step 3 — Plug in and read off

Combining: sample complexity $m = O(\varepsilon^{-1} \cdot (n/\gamma^2) \cdot \text{poly-log})$, which gives Theorems 1 and 2 above.

**Algorithm:** pick any $\sigma$ consistent with the training data (i.e., satisfying $|\text{Tr}(E_i\sigma) - \hat{p}_i| \leq \eta$ for all $i$). Equivalently, solve an SDP over PSD matrices of trace 1. This is information-theoretically guaranteed to work; it's computationally expensive (exponential in $n$ for the SDP) but that's expected — the learning-theoretic result is about sample complexity, not computation.

---

## Application 1 — One-way quantum communication complexity

**Theorem 1.4:** For any (partial or total) Boolean function $f(x, y)$ with Alice input length $N$ and Bob input length $M$:
$$R^1(f) = O(M \cdot Q^1(f))$$
where $R^1$ is randomised one-way communication complexity and $Q^1$ is quantum one-way communication complexity.

**Proof sketch:** Take any $k$-qubit quantum one-way protocol. For Alice's message $\rho_x$, Bob's measurement depends on his input $y$; define the $M$-input distribution over measurements from Bob's strategy. Alice can approximate the learning model: she samples a random subset $S$ of Bob inputs from the uniform distribution, simulates the acceptance probabilities for each, and sends $O(M \cdot k)$ classical bits encoding these approximate values. Bob uses the learnability theorem to recover the information he needs from Alice's message. The $M$ factor comes from the domain size of Bob's measurements.

**Significance:** previously only $D^1(f) = O(M \cdot Q^1(f))$ was known (Klauck). This upgrades to randomised classical, which is a strictly stronger statement. In particular, for $M = O(1)$ (Bob has a small input), quantum one-way protocols offer no more than a constant factor over randomised classical.

---

## Application 2 — Verification of quantum advice

**Theorem 1.5 (BQP/qpoly $\subseteq$ PP/poly):**

Any problem solvable by a polynomial-time quantum algorithm with polynomial-length quantum advice can also be solved by a PP algorithm with polynomial-length classical advice.

**Proof sketch:**
- Fix a BQP/qpoly language $L$ with advice state $|\psi_n\rangle$ for inputs of length $n$.
- For each input $x$ the quantum verifier applies a POVM $E_x$ to $|\psi_n\rangle$ and accepts iff the outcome is 1.
- A PP/poly machine stores a classical approximation $\sigma$ (a polynomial description of an $n$-qubit state) that approximates $|\psi_n\rangle\langle\psi_n|$ on all $2^n$ measurements $\{E_x\}_{x \in \{0,1\}^n}$ simultaneously to error $\gamma$.
- The learnability theorem with $m = O(n/\gamma^2 \varepsilon^2)$ training examples suffices. The total description of $\sigma$ is polynomial in $n$ (up to precision), and it can be verified classically; the computation uses a PP oracle to evaluate $\text{Tr}(E_x \sigma)$ for each $x$.
- PP can simulate the acceptance probability computation, and PP/poly has the classical advice $\sigma$ encoding the learned state.

**Historical note:** Aaronson had previously shown BQP/qpoly $\subseteq$ PostBQP/poly and then PostBQP = PP; this paper gives a more direct construction with better parameters.

---

## Connection to shadow tomography (Aaronson 2018)

The 2006 paper gives an *offline* learning algorithm: see all training examples at once, then find a consistent hypothesis. The 2018 shadow tomography paper solves the harder *online* problem: a sequence of gentle, non-disturbing measurements, each answering a fresh query while keeping $\rho$ approximately intact. The sample complexity in 2018 is polylogarithmic in $M$ (the number of queries), which is exponentially better than the 2006 bound when the number of queries is large. The key extra ingredient in 2018 is an online learning algorithm that avoids disturbing the state — the "learnability" result here doesn't worry about disturbance at all, since it just wants to find a classical description.

See [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]].

---

## Key comparison table

| Paper | Task | Copies required | Computational efficiency |
|---|---|---|---|
| Standard tomography | Reconstruct $\rho$ | $\Theta(4^n/\varepsilon^2)$ | Yes (in principle) |
| **This paper (2006)** | Predict $\text{Tr}(E\rho)$ for $E \sim D$ | PV: $\tilde{O}(n/(\varepsilon\gamma^2))$ via the improved theorem; SB: weaker one-shot bounds, roughly $\tilde{O}(n/(\gamma^8\varepsilon^4))$ from the stated theorem | No (SDP exponential) |
| Aaronson (2018) | Predict online, gently | $\tilde{O}(\varepsilon^{-4}\log^4 M \log D)$ | No (in general) |
| Huang-Kueng-Preskill (2020) | Classical shadows, fixed obs. | $O(\log M / \varepsilon^2)$ for fixed observables under the classical-shadows setting | Yes (measurement + classical post-processing are efficient) |

---

## Limits and caveats

- No efficient algorithm: the SDP finds the consistent hypothesis but runs in time exponential in $n$. The learning-theoretic bound says nothing about computational efficiency, and it's not known how to make this computationally tractable in general.
- Distribution-specific: the guarantee is relative to the unknown distribution $D$. If $D$ is adversarially chosen after seeing the training set, the bounds don't apply. (Standard PAC assumption.)
- Single-bit model has markedly worse parameter dependence than the probability-value model. The stated SB theorem gives roughly $\tilde{O}(n/(\gamma^8\varepsilon^4))$, while the improved PV theorem is roughly $\tilde{O}(n/(\varepsilon\gamma^2))$. So the one-shot setting is much less sample-efficient when high precision is needed.
- The fat-shattering bound uses the QRAC lower bound. This is a statement about quantum random access codes, not about computational hardness — so no hardness assumptions are needed.
- The $O(M \cdot Q^1(f))$ communication bound is for one-way protocols only. For multi-round protocols the analogous statement is not established.

---

## Reusable ideas

1. **Fat-shattering via quantum random access codes.** To bound the learnability of any quantum-state-parameterised function class, fat-shatter a set of $k$ measurements from $\mathcal{C}_n$ and use Ambainis's QRAC lower bound to get $k = O(n/\gamma^2)$. See [[Fat-Shattering Dimension of Quantum States via Random Access Codes]].

2. **PAC learning quantum states.** The p-concept formulation: $F_\rho(E) = \text{Tr}(E\rho)$ is a real-valued concept, and the class $\{F_\rho\}$ is PAC-learnable from $O(n)$ samples. Can be extended to any parameterised family of quantum states with bounded fat-shattering dimension.

3. **Classical simulation of quantum advice via tomography.** Any quantum advice state $|\psi_n\rangle$ can be replaced by a polynomial-length classical description that approximates all acceptance probabilities, giving a PP/poly simulation. This is a general technique for "dequantising" advice.

---

## Cross-links

### Paper notes
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — the online/gentle-measurement successor to this work
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] — later Aaronson work; the block-multilinear polynomial trick has structural parallels
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — BQP model used throughout
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] — BQP/qpoly and advice classes

### Trick cards
- [[Fat-Shattering Dimension of Quantum States via Random Access Codes]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
- [[Postselected Hypothesis Update (Multiplicative Weights)]] — the multiplicative-weights idea used in Aaronson's earlier postselected learning; shadow tomography's core algorithm
- [[Gentle Measurement Lemma]] — relevant to the disturbance-free measurement setting that shadow tomography extends to
