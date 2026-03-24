> **Source:** Scott Aaronson and Alex Arkhipov, *The Computational Complexity of Linear Optics*, arXiv:1011.3245, Proceedings of the 43rd Annual ACM Symposium on Theory of Computing (STOC 2011), pp. 333–342
> **Links:** [arXiv](https://arxiv.org/abs/1011.3245) · [ACM DL](https://dl.acm.org/doi/10.1145/1993636.1993682)
> **Tags:** #quantum-supremacy #BosonSampling #complexity-theory #polynomial-hierarchy #sampling #permanent #linear-optics #foundational

---

## The computational problem

**BosonSampling** (exact version): Given a description of an $m \times m$ linear-optical network $U$ (implementable by beamsplitters and phaseshifters), prepare the input state $|1_n\rangle = |1,1,\ldots,1,0,\ldots,0\rangle$ (one photon in each of the first $n$ modes), apply $U$, and measure in the photon-number basis. Output a sample from the resulting distribution $\mathcal{D}_A$ over occupation vectors $S \in \Phi_{m,n}$ (where $\Phi_{m,n}$ is the set of vectors of nonneg integers summing to $n$).

**Approximate BosonSampling**: Given $U$ and $\varepsilon > 0$, sample from a distribution $\mathcal{D}'_A$ with $\|\mathcal{D}'_A - \mathcal{D}_A\|_{\text{TV}} \leq \varepsilon$ in time $\text{poly}(|A|, 1/\varepsilon)$.

The state space has dimension $M = \binom{m+n-1}{n}$, which grows exponentially in $n$ for $m = \Theta(n^2)$.

The key mathematical connection: for the collision-free case (each output mode receives at most one photon), the probability of outcome $T = (t_1, \ldots, t_n)$ is

$$\Pr[T] = \frac{|\text{Per}(U_{T,[n]})|^2}{s_1! \cdots s_m!}$$

where $U_{T,[n]}$ is the $n \times n$ submatrix of $U$ with rows indexed by $T$ and columns $1,\ldots,n$, and $\text{Per}(\cdot)$ is the matrix permanent. This is why BosonSampling is hard: the permanent is \#P-hard to compute.

---

## What the paper does

Provides strong evidence that BosonSampling cannot be efficiently classically simulated — even approximately. The exact case reduces to classical simulation implying $\text{P}^{\#\text{P}} = \text{BPP}^{\text{NP}}$. The approximate case requires two conjectures about permanents of Gaussian matrices, but gives a cleaner and arguably more physically relevant hardness argument.

This paper, alongside [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]], is the founding result of the quantum computational supremacy programme. It shows that a sub-universal, near-term-feasible quantum device (linear optics, no adaptivity, no quantum error correction) can perform tasks that are classically intractable under standard complexity assumptions.

---

## The model

A **boson computer** (or linear-optical network) is defined as follows:

1. **Modes and photons:** $m$ modes (e.g., optical waveguides), $n$ identical photons. Standard input state $|1_n\rangle$.
2. **Operations:** Apply an $m \times m$ unitary $U$ implemented by $O(m^2)$ beamsplitters and phaseshifters (Reck decomposition; Lemma 14 in the paper, from [Reck et al. 1994]).
3. **No adaptivity:** No intermediate measurements; the circuit is non-adaptive (this is why it's sub-universal — [[Knill-Laflamme-Milburn (KLM) result|KLM]] showed adaptive linear optics is BQP-universal).
4. **Measurement:** Photon-number measurement at the end. Output the occupation vector $S$.

The occupation-number Hilbert space has basis $\{|s_1, \ldots, s_m\rangle : s_i \geq 0, \sum s_i = n\}$ with dimension $\binom{m+n-1}{n}$. For $m = n^2$, this is exponential in $n$.

---

## Key results

### Theorem 1 (Exact hardness)

> If there exists a polynomial-time classical algorithm that exactly samples from $\mathcal{D}_A$ for every boson computer $A$, then $\text{P}^{\#\text{P}} \subseteq \text{BPP}^{\text{NP}}$.

Equivalently: an efficient classical exact sampler would collapse the polynomial hierarchy. Two proofs are given:

1. **Hashing-based:** An exact sampler can be used (with an NP oracle and universal hashing) to estimate individual output probabilities $\Pr[T]$ to within multiplicative error in BPP${}^{\text{NP}}$. Since $\Pr[T] \propto |\text{Per}(U_{T,[n]})|^2$ and approximating permanents of complex matrices is $\#\text{P}$-hard, this gives $\text{P}^{\#\text{P}} \subseteq \text{BPP}^{\text{NP}}$.

2. **Post-selection/KLM:** Post-selected linear optics = PostBQP = PP (by [[Post-Selection Boosting for Hardness of Sampling]] + the [[Aaronson (2005) PostBQP=PP result]]). An exact classical sampler gives post-BPP $\supseteq$ PP, collapsing PH.

### Theorem 3 (Main Result — Approximate hardness)

> Let $\mathcal{D}_A$ be the output distribution of a boson computer $A$. Suppose there exists a classical algorithm $C$ that, given $A$ and $\varepsilon$, samples from $\mathcal{D}'_A$ with $\|\mathcal{D}'_A - \mathcal{D}_A\|_{\text{TV}} \leq \varepsilon$ in time $\text{poly}(|A|, 1/\varepsilon)$. Then $|\text{GPE}|^2_{\pm}$ is in $\text{BPP}^{\text{NP}}$.

This is the key approximate hardness result. It reduces approximate BosonSampling to the problem of estimating $|\text{Per}(X)|^2$ for Gaussian matrices.

### Problem 2 ($|\text{GPE}|^2_{\pm}$ — Gaussian Permanent Estimation)

> Given $X \sim \mathcal{N}(0,1)_{n \times n}^{\mathbb{C}}$ (i.i.d. complex Gaussians), $\varepsilon, \delta > 0$: estimate $|\text{Per}(X)|^2$ to within additive error $\pm \varepsilon \cdot n!$, with probability $\geq 1 - \delta$ over $X$, in time $\text{poly}(n, 1/\varepsilon, 1/\delta)$.

### Conjecture 5 (Permanent-of-Gaussians Conjecture, PGC)

> $\text{GPE}_{\times}$ (multiplicative-error Gaussian permanent estimation) is $\#\text{P}$-hard.

### Conjecture 6 (Permanent Anti-Concentration Conjecture, PACC)

> There exists a polynomial $p$ such that for all $n$ and $\delta > 0$:
> $$\Pr_{X \sim \mathcal{N}(0,1)_{n \times n}^{\mathbb{C}}}\!\left[|\text{Per}(X)| < \frac{\sqrt{n!}}{p(n, 1/\delta)}\right] < \delta$$

In words: $|\text{Per}(X)|$ does not concentrate near zero — it spreads out over a wide range, comparable to $\sqrt{n!}$.

### Theorem 7 (Equivalence under PACC)

> If PACC holds, then $|\text{GPE}|^2_{\pm}$ and $\text{GPE}_{\times}$ are polynomial-time equivalent.

Combining Theorems 3 and 7 with Conjecture 5: if PGC and PACC both hold, then approximate BosonSampling is classically intractable (assuming $\text{P}^{\#\text{P}} \not\subseteq \text{BPP}^{\text{NP}}$).

---

## The approximate hardness argument in detail

The gap between exact and approximate hardness is real: a single tiny output probability can be corrupted by an adversarial approximate sampler without affecting total variation distance much. The paper bridges this with a two-step argument.

**Step 1: Submatrix-Gaussian connection.** For a Haar-random $m \times m$ unitary $U$, any $n \times n$ submatrix is close (in variation distance) to $\mathcal{N}(0, 1/m)_{n \times n}^{\mathbb{C}}$ when $n \leq m^{1/6-\varepsilon}$ (roughly). So output probabilities of a random boson computer are distributed like rescaled permanents of Gaussian matrices. See [[Submatrix-of-Haar-Unitary to Gaussian Approximation]].

**Step 2: Hiding the hard instance.** To estimate $|\text{Per}(X)|^2$ for a particular $X$, embed $X$ as a (rescaled) submatrix of a random $U$, then run the approximate classical sampler on the corresponding boson computer. With good probability the output encodes $|\text{Per}(X)|^2$ as a probability of a specific outcome. The approximate sampler cannot know which output to corrupt (since $U$ is random), so with high probability that specific probability is faithfully represented. Use hashing + NP oracle to extract it.

This is an instance of the [[Gaussian Permanent Hiding via Haar Random Embedding]] trick.

---

## The permanent connection

For the collision-free case (standard when $m \gg n$):

$$\Pr[\text{output } T] = \frac{|\text{Per}(A_T)|^2}{\prod_i s_i!}$$

where $A_T$ is the $n \times n$ submatrix of $U$ selecting rows $T$ and columns $[n]$, and $s_i$ are the occupation numbers. For the uniform anti-bunching case ($s_i \in \{0,1\}$), the denominator is 1.

This is a direct consequence of the bosonic [[Permanent Formula for Bosonic Amplitudes|permanent formula for bosonic amplitudes]]: the amplitude for $n$ indistinguishable bosons distributed from input modes $r_1, \ldots, r_n$ to output modes $t_1, \ldots, t_n$ through unitary $U$ is $\text{Per}(U_{\{t_i\}, \{r_i\}}) / \sqrt{s_1! \cdots s_m! \cdot t_1! \cdots t_m!}$.

---

## Comparison with prior work

| Result | Model | Error model | Hardness assumption |
|---|---|---|---|
| [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes\|Bremner-Jozsa-Shepherd (2011)]] | IQP (commuting gates) | Multiplicative | post-IQP = PP (unconditional) |
| **Aaronson-Arkhipov (2011)** (exact) | Linear optics, $n$ photons | Exact | \#P-hardness of permanents |
| **Aaronson-Arkhipov (2011)** (approx) | Linear optics, $n$ photons | TV distance | PGC + PACC (conjectural) |
| Valiant (1979) | — | — | \#P-hardness of permanent (exact, unconditional) |

The main novelty over Bremner-Jozsa-Shepherd is that BosonSampling gives a physically realizable model (linear optics) and the approximate hardness argument connects to a specific well-studied algebraic object (the permanent), grounding the conjectures in something more concrete than the IQP post-selection argument. On the other hand, IQP's exact hardness is cleaner (no extra conjectures needed for the multiplicative case).

---

## Limits and caveats

- **PACC and PGC are open conjectures.** The approximate hardness argument requires both. The PACC is considered likely true (the permanent of a Gaussian matrix is expected to have similar anti-concentration to a Gaussian itself, by Ryan O'Donnell's analysis), but neither conjecture has been proved.
- **$m \gg n$ needed.** The Submatrix-Gaussian approximation requires $n \leq m^{1/6}$ approximately. The standard recommendation $m = O(n^2)$ is used throughout. In this regime, collisions (multiple photons in one output mode) are rare.
- **Exact vs approximate gap.** The exact hardness argument breaks under any approximation error, which is why PGC + PACC are needed. Real experiments have noise; connecting noisy BosonSampling hardness to the theory requires additional work (and has generated a cottage industry since 2012).
- **Not universal quantum computation.** The model is sub-BQP. Hardness of sampling from it doesn't imply BQP $\neq$ BPP.
- **Classical simulation progress.** Since 2011, substantial classical algorithm work (tensor network methods, Clifford + few-photon tricks) has made large-$n$ exact simulation harder to verify experimentally. The current classical simulation frontier for approximate BosonSampling sits around $n \approx 50$–100 photons depending on approximation fidelity.
- **Experimental verification.** Verifying that an experimental device is actually sampling from $\mathcal{D}_A$ (rather than from some classically easy distribution) is itself a hard problem, largely orthogonal to the complexity theory.

---

## Reusable ideas

1. **[[Post-Selection Boosting for Hardness of Sampling]]** — post-$\mathcal{C}$ = PP + Toda → PH collapse from classical simulation. (Shared with Bremner-Jozsa-Shepherd; the BosonSampling version uses PostBQP = PP via KLM.)

2. **[[Gaussian Permanent Hiding via Haar Random Embedding]]** — embed a hard Gaussian matrix as a submatrix of a Haar-random unitary, map it to a boson computer, and use randomness to prevent adversarial corruption. The approximate sampler can't know which outcome to corrupt, so the permanent is faithfully represented with high probability.

3. **[[Submatrix-of-Haar-Unitary to Gaussian Approximation]]** — an $n \times n$ submatrix of an $m \times m$ Haar-random unitary is close in total variation to an i.i.d. complex Gaussian matrix (scaled by $1/\sqrt{m}$), when $n \ll m^{1/2}$. Quantitative version requires $n \leq m^{1/6}$ for a useful bound. This is the bridge between the abstract linear-optics model and the Gaussian permanent problem.

4. **[[Permanent Formula for Bosonic Amplitudes]]** — the amplitude/probability for indistinguishable bosons through a linear-optical unitary is the permanent of the corresponding submatrix. This is the foundational connection between quantum optics and computational complexity.

---

## References within this paper

- **Valiant (1979)** — \#P-hardness of the permanent. The foundational result making BosonSampling hard.
- **Reck, Zeilinger, Bernstein, Bertani (1994)** — any unitary can be decomposed into $O(m^2)$ beamsplitters. Used for Lemma 14.
- **Knill, Laflamme, Milburn (KLM) (2001)** — linear optics with adaptive measurements is BQP-universal; postselected linear optics = PostBQP. Central to the exact hardness proof.
- **Aaronson (2005)** — PostBQP = PP. Used in both proofs of Theorem 1.
- **[[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]]** — concurrent work, IQP model, same style of complexity argument. The two papers appeared essentially simultaneously and are often cited together.
- **Aaronson (2011) [arXiv:1009.5104]** — sampling/searching equivalence theorem (Theorems 12–13 in this paper).

---

## Cross-links

### Paper notes
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — concurrent; IQP vs linear optics; same PH-collapse template

### Trick cards
- [[Post-Selection Boosting for Hardness of Sampling]]
- [[Gaussian Permanent Hiding via Haar Random Embedding]]
- [[Submatrix-of-Haar-Unitary to Gaussian Approximation]]
- [[Permanent Formula for Bosonic Amplitudes]]
