> **Source:** Ryan Babbush, Dominic W. Berry, Hartmut Neven, *Quantum Simulation of the Sachdev-Ye-Kitaev Model by [[Asymmetric Qubitization]]*, arXiv:1806.02793, Phys. Rev. A **99**, 040301(R) (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1806.02793) · [Journal](https://doi.org/10.1103/PhysRevA.99.040301)
> **Tags:** #quantum-simulation #SYK-model #qubitization #LCU #fault-tolerant #T-complexity #holography #Majorana-fermions #quantum-signal-processing

---

## The computational problem

Simulate the time evolution $e^{-iHt}$ of the Sachdev-Ye-Kitaev (SYK) model

$$H = \frac{1}{4 \cdot 4!} \sum_{p,q,r,s=0}^{N-1} J_{pqrs}\, \gamma_p \gamma_q \gamma_r \gamma_s$$

where $\gamma_p$ are Majorana fermion operators and the couplings $J_{pqrs}$ are real-valued, drawn from a Gaussian distribution with variance $\sigma^2 = 3! J^2 / N^3$. The model has $L = N^4$ terms and $N$ Majorana modes (requiring $N/2$ qubits under Jordan-Wigner). Simulate for time $t$ to precision $\varepsilon$ in operator norm.

The SYK model is interesting because it's maximally chaotic (same Lyapunov exponent as black holes in Einstein gravity) and has an approximate conformal symmetry at low temperature suggesting a holographic dual to AdS$_2$ gravity. The simulation problem is motivated by studying properties of the thermal state that are analytically intractable — e.g., sampling the density of states via quantum phase estimation.

---

## What the paper does

Achieves gate complexity $O(N^{7/2}t + N^{5/2}t\,\mathrm{polylog}(N/\varepsilon))$ for simulating SYK dynamics — an exponential improvement in $1/\varepsilon$ and a large polynomial improvement in $N$ and $t$ over the prior Lie-Trotter approach of García-Álvarez et al. (2017), which scaled as $O(N^{10} t^2/\varepsilon)$.

The paper introduces **asymmetric qubitization**: a variant of [[Qubitization (Quantum Walk for Spectral Encoding)|standard qubitization]] where the Hamiltonian is block-encoded using *two different* state preparation oracles $A$ and $B$ rather than a single PREPARE oracle. The encoding relation becomes $H/\lambda = \langle B | V | A \rangle$ instead of $H/\lambda = \langle G | U | G \rangle$. This relaxation is what makes the SYK simulation efficient: $B$ is just Hadamard gates (equal superposition) and $A$ is a random quantum circuit that naturally produces Gaussian-distributed amplitudes matching the SYK couplings.

This is a short, elegant paper. The idea is simple but the application is well-chosen — the match between random circuit amplitudes and Gaussian-distributed couplings is almost too clean.

---

## The algorithm / construction

### 1. Asymmetric qubitization framework

Standard [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] (Low & Chuang) requires a single state preparation oracle $G$ such that $\langle G | U | G \rangle = H/\lambda$, where $U = \mathrm{SELECT}$ applies the Hamiltonian terms controlled on an index register. The PREPARE oracle must encode amplitudes proportional to $\sqrt{w_\ell}$.

The asymmetric variant uses two oracles: $A|0\rangle = |A\rangle$ with amplitudes $\alpha_\ell$ and $B|0\rangle = |B\rangle$ with amplitudes $\beta_\ell$, plus a signal oracle $V$, such that

$$\frac{H}{\lambda} = \langle B | V | A \rangle = \sum_\ell \alpha_\ell \beta_\ell^* H_\ell$$

This requires $\lambda \alpha_\ell \beta_\ell^* = w_\ell$ for each term. By Cauchy-Schwarz on normalized states $|A\rangle$ and $|B\rangle$:

$$\lambda \geq \sum_\ell |w_\ell|$$

with equality when $|\alpha_\ell| = |\beta_\ell|$ (the symmetric case). The overhead from asymmetry is the ratio $\sqrt{\langle |w_\ell|^2 \rangle} / \langle |w_\ell| \rangle$. For Gaussian-distributed $w_\ell$ (as in SYK), this ratio is only $\sqrt{\pi/2} \approx 1.25$ — negligible.

### 2. Making asymmetric qubitization self-inverse

Standard qubitization needs $U$ to be self-inverse (a reflection). With asymmetric oracles, $B^\dagger V A$ is not self-inverse. The fix: add an ancilla qubit and construct

$$U = \begin{pmatrix} 0 & A^\dagger V B \\ B^\dagger V A & 0 \end{pmatrix} = \begin{pmatrix} A^\dagger & 0 \\ 0 & B^\dagger \end{pmatrix} \begin{pmatrix} 0 & V \\ V & 0 \end{pmatrix} \begin{pmatrix} A & 0 \\ 0 & B \end{pmatrix}$$

The ancilla controls which preparation ($A$ or $B$) is applied. The middle block is $V$ plus a NOT on the ancilla — self-inverse since $V$ is self-inverse. The reflection $R = 2|G\rangle\langle G| - I$ uses $G$ = Hadamard on the new ancilla. The walk operator $W = RU$ then has eigenvalues $e^{\pm i \arccos(h/\lambda)}$ as in standard qubitization, enabling [[Qubitization (Quantum Walk for Spectral Encoding)|quantum signal processing]] for time evolution.

The paper also describes an alternative via oblivious amplitude amplification (Appendix A) but notes it requires $3\times$ more applications of $V$, so the main-text construction is preferred.

### 3. State preparation oracles for SYK

**$B$ oracle:** Equal superposition over $L = N^4$ indices. Implementation: $4\log N$ Hadamard gates. Cost: $O(\log N)$.

**$A$ oracle:** A random orthogonal quantum circuit on $4\log N$ qubits. The output amplitudes $\alpha_\ell$ are Gaussian distributed with zero mean (a known property of random orthogonal evolutions). These naturally match the Gaussian couplings $J_{pqrs}$ of the SYK model up to an overall normalization absorbed into $\lambda$.

The circuit size required for sufficient convergence to the Gaussian distribution is $O(\mathrm{polylog}(N/\varepsilon))$, based on known results for convergence of random circuits to approximate 2-designs (Harrow & Low 2009; Harrow & Mehraban 2018) and numerical evidence for convergence to Porter-Thomas distributions (Boixo, Babbush et al. 2018).

The connection is appealing: the SYK model is maximally chaotic, and random quantum circuits are a standard model for quantum chaos. The fact that random circuits naturally generate the right coupling distribution isn't a coincidence — it's a reflection of the same universality.

### 4. SELECT oracle (controlled Majorana application)

The SELECT oracle implements

$$V |p\rangle|q\rangle|r\rangle|s\rangle|\psi\rangle \mapsto |p\rangle|q\rangle|r\rangle|s\rangle\,\gamma_p\gamma_q\gamma_r\gamma_s|\psi\rangle$$

Each Majorana operator $\gamma_\ell$ maps to $X_\ell Z_{\ell-1} \cdots Z_0$ under Jordan-Wigner. The controlled application of a single Majorana operator (circuit from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]], Section 3B) has T-count $4N - 4$ and uses $\log N$ ancillae, with total qubit count $N + 2\log N + 1$.

Four applications of this primitive implement the full four-body $\gamma_p\gamma_q\gamma_r\gamma_s$. Total T-count for SELECT: $16N - 16$, using $\log N$ ancillae.

### 5. Quantum signal processing for time evolution

The walk operator $W = RU$ satisfies $\langle G | W^n | G \rangle = T_n(H/\lambda)$ where $T_n$ is the $n$th Chebyshev polynomial. Using the Jacobi-Anger expansion

$$e^{-iHt} = J_0(-\lambda t) + 2\sum_{n=0}^\infty i^n J_n(-\lambda t) T_n(H/\lambda)$$

quantum signal processing constructs the required polynomial of $W$ with order

$$M = O\!\left(\lambda t + \frac{\log(1/\varepsilon)}{\log\log(1/\varepsilon)}\right)$$

For large $\lambda t$, the more precise asymptotic (derived in Appendix B via Airy-function asymptotics of Bessel functions in the transition region) is

$$M \approx \lambda t + \frac{3^{2/3}}{2}(\lambda t)^{1/3} \log^{2/3}(1/\varepsilon)$$

### 6. Total complexity

Each step of quantum signal processing uses one application each of $A$, $B$, and $V$. With $C_A = O(\mathrm{polylog}(N/\varepsilon))$, $C_B = O(\log N)$, and $C_V = O(N)$, and $\lambda = O(N^{5/2}J)$ (from the Gaussian variance of $J_{pqrs}$):

$$\text{Total gate complexity} = O\!\left(N^{7/2}t + N^{5/2}t\,\mathrm{polylog}(N/\varepsilon)\right)$$

---

## Key results

**Main result:**

$$\boxed{O\!\left(N^{7/2}Jt + N^{5/2}Jt\,\mathrm{polylog}(N/\varepsilon)\right) \text{ gates}}$$

**LCU 1-norm:**

$$\lambda = N^{5/2}J \cdot \frac{\sqrt{3!}}{4 \cdot 4!} \approx 0.0102\, N^{5/2} J$$

**Leading-order T-count:**

$$2\sqrt{6}\, N^{7/2} Jt$$

**Concrete estimates:**
- $N = 100$: T-count $< 10^7 Jt$
- $N = 200$: T-count $< 10^8 Jt$

These should be compared with $\sim 10^{12}$ T gates for FeMoco simulation and $\sim 10^9$ T gates for interesting solid-state electronic structure or Heisenberg model problems. The SYK model is among the most viable early applications of surface-code quantum computers.

**Per-component costs:**

| Component | Cost | Notes |
|---|---|---|
| $A$ (random circuit) | $O(\mathrm{polylog}(N/\varepsilon))$ | Random orthogonal circuit |
| $B$ (Hadamards) | $O(\log N)$ | $4\log N$ Hadamard gates |
| $V$ (SELECT) | $16N - 16$ T gates | 4 controlled Majorana applications |
| $\lambda$ | $O(N^{5/2}J)$ | From Gaussian coupling variance |
| Signal processing order | $O(\lambda t + \log(1/\varepsilon)/\log\log(1/\varepsilon))$ | Jacobi-Anger truncation |

---

## Comparison with prior work

| Method | Gate complexity | Precision scaling | Reference |
|---|---|---|---|
| Lie-Trotter (García-Álvarez et al. 2017) | $O(N^{10} t^2/\varepsilon)$ | $O(1/\varepsilon)$ | PRL **119**, 040501 |
| **Asymmetric qubitization (this work)** | $O(N^{7/2}t + N^{5/2}t\,\mathrm{polylog}(N/\varepsilon))$ | $O(\mathrm{polylog}(1/\varepsilon))$ | PRA **99**, 040301(R) |

The improvement factors:
- $N$: from $N^{10}$ to $N^{7/2}$ — a factor of $N^{13/2}$. At $N = 100$, this is $\sim 10^{13}$.
- $t$: from $t^2$ to $t$ — quadratic improvement.
- $\varepsilon$: from $1/\varepsilon$ to $\mathrm{polylog}(1/\varepsilon)$ — exponential improvement.

The prior approach was impractical for anything beyond toy sizes. This paper makes $N > 100$ SYK simulation plausible on a fault-tolerant machine.

---

## Limits / caveats

- **Random circuit convergence.** The $O(\mathrm{polylog}(N/\varepsilon))$ cost for the $A$ oracle assumes rapid convergence of random orthogonal circuit amplitudes to a Gaussian distribution. The paper cites numerical evidence (Boixo et al. 2018) and theoretical bounds for approximate 2-designs, but there's no rigorous proof that $O(\mathrm{polylog}(N/\varepsilon))$ suffices for the specific amplitude-distribution convergence needed. The authors acknowledge this with "we will conservatively assume."

- **Coupling-dependent simulation time.** The T-count scales as $N^{7/2}Jt$, so the physical time $t$ that matters is $Jt$ (dimensionless). For the SYK model to exhibit its interesting holographic properties, $J$ must be large (strong coupling), which pushes up $\lambda t$. The paper doesn't estimate what values of $Jt$ are needed for physically interesting simulations.

- **No initial state preparation.** The paper addresses [[Hamiltonian simulation]] only. To study the thermal state or compute out-of-time-order correlators, one also needs to prepare the initial state (e.g., a thermal/Gibbs state), which has its own complexity. This is a separate open problem.

- **Asymmetry overhead is mild here but not always.** The $\sqrt{\pi/2}$ overhead from asymmetric qubitization is specific to Gaussian-distributed couplings. For other distributions, the ratio $\sqrt{\langle |w_\ell|^2\rangle}/\langle |w_\ell|\rangle$ could be much worse. The technique is most natural when there's a distribution-theoretic match between random circuit amplitudes and the Hamiltonian couplings.

- **Jordan-Wigner cost.** The SELECT oracle uses Jordan-Wigner strings of length up to $N$, contributing the $O(N)$ per-oracle cost. Other fermion encodings (e.g., Bravyi-Kitaev) could potentially reduce the constant factor but not the asymptotic scaling.

---

## Reusable ideas

1. [[Asymmetric Qubitization]] — Use two different state preparation oracles $A$ and $B$ in the [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] framework, encoding $H/\lambda = \langle B | V | A \rangle$. Construct a self-inverse walk operator by introducing an ancilla that selects between $A$ and $B$. Overhead factor: $\sqrt{\langle |w_\ell|^2\rangle}/\langle |w_\ell|\rangle$ over symmetric qubitization.

2. [[Random Circuit State Preparation (Gaussian Amplitudes)]] — Use a random orthogonal quantum circuit to prepare a state whose amplitudes are Gaussian distributed. Matches the coupling distribution of models with random Gaussian couplings (SYK, random matrix models). Circuit size $O(\mathrm{polylog}(L/\varepsilon))$ for $L$-dimensional Hilbert space.

---

## References within this paper

| Reference | What it is | Vault note? |
|---|---|---|
| Maldacena (1999) | AdS/CFT correspondence | No |
| Kitaev (2015) | Original SYK model; maximal chaos result | No |
| Sachdev & Ye (1993) | Original SY model that SYK generalizes | No |
| García-Álvarez et al. (2017), PRL | Prior Trotter-based SYK simulation: $O(N^{10}t^2/\varepsilon)$ | No |
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2016)]], arXiv:1610.06546 | [[Qubitization (Quantum Walk for Spectral Encoding)]] — the framework this paper extends | [[Qubitization (Quantum Walk for Spectral Encoding)]] |
| Low & Chuang (2017), PRL | Quantum signal processing — polynomial transformations of walk operators | No |
| [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] | LCU query model for [[Hamiltonian simulation]] | No |
| Babbush, Gidney et al. (2018), PRX [ref 21] | Controlled Majorana operator circuit (Section 3B); source of the SELECT implementation used here | [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] |
| Gidney (2017), arXiv:1709.06648 | Tofolli gate compilation to Clifford+T | No |
| Boixo, [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush et al. (2018)]], Nature Physics | Random circuit sampling; Porter-Thomas convergence evidence | No |
| [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]], FOCS | Product of reflections → rotation (quantum walk theory) | No |
| Berry, Childs, Kothari (2015), FOCS | [[Truncated Taylor Series Simulation]] | [[Truncated Taylor Series Simulation]] |
| Childs, Kothari, Somma (2017), SIAM | LCU simulation optimality | No |
| Fowler et al. (2012), PRA | Surface code — motivates T-gate minimization | No |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush et al. (2018)]], PRX, 1711.07629 [ref 30] | Low-depth materials simulation | [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] |
| Berry, Gidney, Motta, McClean, Babbush (2019) [ref 29] | Improved qubitization resource estimates | [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] |
| Childs, Maslov et al. (2018), PNAS [ref 31] | Heisenberg model simulation | No |
| Harrow & Low (2009); Harrow & Mehraban (2018) | Random circuit convergence to approximate designs | No |

---

## Cross-links

### Paper notes
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — the symmetric qubitization framework that this paper generalizes; asymmetric qubitization keeps the same walk-operator geometry but splits PREPARE into two oracles
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT framework; asymmetric qubitization fits as a block-encoding variant where the signal oracle and the two preparation circuits define an asymmetric standard-form encoding
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — concurrent Babbush-Berry paper applying qubitization to arbitrary molecular orbital bases via QROAM and low-rank factorization; same year, different application domain (chemistry vs. holographic physics); introduced as ref [29]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — provides the controlled Majorana operator circuit (Section 3B) used directly as the SELECT primitive here; also the canonical reference for [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] applied to chemistry
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — concurrent work by the same group (Babbush, Berry, Neven); uses [[Interaction Picture Simulation (Kinetic Frame)|interaction picture]] rather than qubitization; different application domain (chemistry vs. holographic physics) but same intellectual program of pushing beyond Trotter
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — cited as ref [30] for context on surface-code T-gate resource estimates for condensed matter problems
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — applies the qubitized walk framework to combinatorial optimization; introduces stroboscopic adiabatic walk and compiles three QSA variants with constant-factor Toffoli counts; shares the Sanders co-authorship
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|Berry, Su, Babbush et al 2024]]
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin 2024]]
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes|Costa, An, Babbush, Berry 2023]]

### Trick cards
- [[Asymmetric Qubitization]] — the main new technique from this paper
- [[Random Circuit State Preparation (Gaussian Amplitudes)]] — using random circuits to generate the LCU state preparation for Gaussian-distributed couplings
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the symmetric version that this paper generalizes
- [[Truncated Taylor Series Simulation]] — the LCU framework underlying the signal-processing approach
- [[Unary Iteration]] — used inside the controlled Majorana circuit from ref [21]
