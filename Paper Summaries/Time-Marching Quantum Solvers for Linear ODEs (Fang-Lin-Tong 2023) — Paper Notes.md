> **Source:** Di Fang, Lin Lin, and Yu Tong, *Time-marching based quantum solvers for time-dependent linear differential equations*, arXiv:2208.06941, Quantum **7**, 955 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2208.06941) · [Quantum](https://quantum-journal.org/papers/q-2023-03-20-955/)
> **Tags:** #quantum-algorithm #differential-equations #time-marching #QSVT #block-encoding #singular-value-amplification #compression-gadget

---

## The problem

Solve the initial-value problem for a homogeneous linear ODE on a quantum computer:

$$\frac{\mathrm{d}}{\mathrm{d}t}|\psi(t)\rangle = A(t)|\psi(t)\rangle, \quad |\psi(0)\rangle = |\psi_0\rangle, \quad t \in [0,T],$$

where $A(t) \in \mathbb{C}^{N \times N}$ is time-dependent, accessed via block-encoding oracles (one per time segment). The goal is to prepare a quantum state $\epsilon$-close (in trace distance) to $|\psi(T)\rangle / \||\psi(T)\rangle\|$.

Unitary dynamics ($A = -iH$) is the special case where $\||\psi(T)\rangle\| = 1$. The general case has $\||\psi(T)\rangle\|$ that can grow or shrink, and the difficulty is captured by the **amplification ratio**:

$$Q := \frac{\prod_{l=1}^{L} \|\Xi_l\|}{\||\psi(T)\rangle\|}, \quad \text{where } \Xi_l = \mathcal{T}e^{\int_{t_{l-1}}^{t_l} A(s)\,ds}.$$

$Q$ measures the gap between the product of short-time propagator norms and the actual final-state norm. For unitary evolution $Q = 1$. For dissipative or growing dynamics, $Q$ can be exponential.

---

## What the paper does

Rehabilitates the **time-marching strategy** for quantum ODE solvers. The naive approach — propagate short-time solutions segment by segment, post-selecting on ancilla measurement after each step — has exponentially vanishing success probability in the number of steps $L$. This paper shows how to fix that using two techniques:

1. **Uniform singular value amplification (USVA):** after each short-time block-encoding, amplify the encoded operator's singular values to normalise it, reducing the norm ratio from $\alpha_l / \|\bar{\Xi}_l\|$ (which can be large) to $O(1)$.
2. **Compression gadget:** compose all $L$ amplified block-encodings into a single block-encoding of the full propagator, using only $O(\log L)$ extra ancilla qubits.

The result: the success probability drops to $\Omega(Q^{-2})$ rather than $\Omega(\prod_l \|\Xi_l\|^{-2})$, and a single round of [[Standard Amplitude Amplification|amplitude amplification]] with $O(Q)$ repetitions brings it to $2/3$.

**My assessment:** This is a genuinely useful alternative to [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|QLSA-based]] ODE solvers. The three claimed advantages are real: it handles non-diagonalisable $A(t)$, needs only bounded variation (not smoothness), and uses fewer initial-state queries. The amplification ratio $Q$ replaces the condition number $\kappa$ as the hardness parameter, and Theorem 10 proves this dependence is tight (via reduction to unstructured search). The main open question — whether $O(T^2)$ matrix-query scaling can match QLSA's $O(T)$ — remains unresolved and interesting. The paper also connects nicely to the Fang-Lin group's earlier [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes|qHOP]] work, since the first-order Magnus integrator they analyse is essentially qHOP for general (non-Hamiltonian) dynamics.

---

## The algorithm / construction

### Step 1: Short-time propagator via truncated Dyson series (Lemma 5)

Partition $[0,T]$ into $L$ segments with $t_l - t_{l-1} \leq (2\alpha)^{-1}$ (where $\alpha$ is the block-encoding normalisation). On each segment, approximate $\Xi_l = \mathcal{T}e^{\int_{t_{l-1}}^{t_l} A(s)\,ds}$ by a truncated Dyson series, using the same machinery as [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]].

This produces an $(\alpha_l, m_l, \epsilon_l\|\Xi_l\|)$-block encoding $U_l$ of $\Xi_l$, using $O(\log(1/\epsilon') \cdot \log\log(1/\epsilon'))$ queries to the time-dependent matrix encoding oracle (MAT) per segment. The normalisation $\alpha_l = O(1)$ when segments are short enough.

### Step 2: Uniform singular value amplification (Lemma 2)

Given $U_l$ as an $(\alpha_l, m_l, 0)$-block encoding of $\bar{\Xi}_l$, construct a $(\|\bar{\Xi}_l\|/(1-\delta), m_l+1, \epsilon'\|\bar{\Xi}_l\|)$-block encoding of $\bar{\Xi}_l$ using

$$O\!\left(\frac{\alpha_l}{\delta\|\bar{\Xi}_l\|} \cdot \log\frac{\alpha_l\|\bar{\Xi}_l\|}{\epsilon'}\right)$$

applications of controlled-$U_l$ and its inverse. The construction uses [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] with an odd polynomial that approximates $x/\|x\|$ on the relevant singular value interval.

The effect: each amplified block-encoding $\tilde{U}_l$ has normalisation $\alpha_l' \approx \|\bar{\Xi}_l\|$ instead of $\alpha_l \gg \|\bar{\Xi}_l\|$. This is the step that prevents the exponential success-probability decay.

### Step 3: Compression gadget (Lemma 3)

Given block-encodings $\tilde{U}_1, \ldots, \tilde{U}_L$ of operators $\tilde{\Gamma}_1, \ldots, \tilde{\Gamma}_L$, construct a block-encoding of the product $\tilde{\Gamma}_L \cdots \tilde{\Gamma}_1$ with:
- Normalisation: $\alpha_{\text{comp}} = \prod_l \alpha_l'$
- Extra ancilla: $\lceil \log_2 L \rceil + 1$ qubits (a coherent counter)
- Uses each $\tilde{U}_l$ exactly once

The gadget (adapted from [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]]) avoids the naive tensor-product construction that would require all $\tilde{U}_l$ simultaneously. Instead, it uses a binary tree of controlled swaps to route the computation through each block-encoding in sequence.

### Step 4: Amplitude amplification

The compressed block-encoding has normalisation $\alpha_{\text{comp}} \approx \prod_l \|\Xi_l\|$. Measuring the ancilla gives success probability $\||\psi(T)\rangle\|^2 / \alpha_{\text{comp}}^2 = \Omega(Q^{-2})$. Apply $O(Q)$ rounds of [[Standard Amplitude Amplification|amplitude amplification]] using the initial-state preparation unitary $U_{\text{init}}$.

---

## Key results

### Theorem 8 (Dyson series solver)

Under the conditions above, prepare a state $\epsilon$-close to $|\psi(T)\rangle / \||\psi(T)\rangle\|$ with probability $\geq 2/3$ using:

$$O\!\left(\alpha^2 T^2 Q \cdot \log(\alpha T Q) \cdot \frac{\log(\alpha T Q \epsilon^{-1})}{\log\log(\alpha T Q \epsilon^{-1})}\right)$$

queries to the MAT oracles, and $O(Q)$ applications of controlled-$U_{\text{init}}$.

For the **sparse matrix** input model ($d$-sparse, $\|A(t)\| \leq 1$, Corollary 9): replace $\alpha$ with $d$ throughout.

### Theorem 10 (Lower bound)

For any $\theta > 0$, there exists an instance where no quantum algorithm can solve the ODE using $O(Q^{1-\theta}\operatorname{poly}(T))$ queries. The proof reduces to unstructured search: set $A = -U_{\text{targ}}$ (the Grover oracle), then the ODE amplifies the marked element exponentially, and $Q = \Theta(\sqrt{N})$. Sub-linear-in-$Q$ query complexity would violate the $\Omega(\sqrt{N})$ search lower bound.

This shows the **linear dependence on $Q$** in Theorem 8 is optimal.

### Theorem 14 (First-order Magnus solver)

Replaces the high-order Dyson series with a simpler first-order [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes|Magnus integrator]] (drop the time-ordering, exponentiate the time-averaged matrix). The implementation uses the contour integral formulation $e^A = \frac{1}{2\pi i}\oint_\Gamma e^z(z-A)^{-1}\,dz$ (discretised by trapezoidal rule, then each resolvent computed via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] matrix inversion).

Query complexity:

$$O\!\left(\alpha T \cdot \max\!\left\{\frac{\alpha_{\text{comm}} T^2 Q}{\epsilon},\, \alpha T\right\} \cdot \log^2\!\left(\frac{\max\{\alpha_{\text{comm}}, \alpha\} T Q}{\epsilon}\right)\right)$$

where $\alpha_{\text{comm}} = \frac{1}{T}\sum_l \frac{1}{2}\int_{t_{l-1}}^{t_l}\mathrm{d}\tau\, \|\int_{t_{l-1}}^{\tau} [A(s)\,\mathrm{d}s, A(\tau)]\|$ measures commutator scaling. When $A(t)$ commutes with itself at all times, $\alpha_{\text{comm}} = 0$ and the complexity matches the Dyson solver. The implementation is simpler — no time-ordering control logic.

---

## Comparison with QLSA-based approaches

| Feature | Time-marching (this paper) | [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes\|QLSA-based]] (Berry 2017, Krovi 2022, etc.) |
|---|---|---|
| Hardness parameter | Amplification ratio $Q$ | Condition number $\kappa$ of discretised system |
| Matrix query scaling | $O(\alpha^2 T^2 Q)$ | $O(\alpha T \kappa)$ (better in $T$) |
| Diagonalisability | Handles non-diagonalisable $A(t)$ | Requires $A(t)$ diagonalisable or special structure |
| Smoothness requirement | Bounded variation only | Higher smoothness for high-order accuracy |
| Initial state queries | $O(Q)$ (independent of $T$, $\alpha$, $\epsilon$) | $O(\kappa \operatorname{polylog}(1/\epsilon))$ (depends on $T$ through $\kappa$) |
| Precision scaling | $\operatorname{polylog}(1/\epsilon)$ | $\operatorname{polylog}(1/\epsilon)$ |
| Optimality | Linear in $Q$ is tight (Thm 10) | Linear in $\kappa$ is tight |

### The three advantages over QLSA

1. **Non-diagonalisable $A(t)$:** QLSA-based methods typically convert the ODE to a linear system via discretisation, then invert. The resulting system's condition number $\kappa_V$ can involve the eigenvector condition number of $A$, which is infinite for non-diagonalisable matrices. Time-marching sidesteps this because $Q$ is defined through propagator norms, not spectral decompositions.

2. **Non-smooth $A(t)$:** The truncated Dyson series achieves $\operatorname{polylog}(1/\epsilon)$ precision scaling requiring only bounded variation of $A(t)$. QLSA-based methods using spectral discretisation (e.g., Childs & Liu, Commun. Math. Phys. 2020) need $r$ derivatives for $r$th-order accuracy.

3. **Fewer initial-state queries:** Time-marching uses $O(Q)$ applications of $U_{\text{init}}$, independent of $T$, $\alpha$, or $\epsilon$. QLSA-based methods use the initial state inside the linear system solve, where the number of applications depends on $\kappa$ (and thus on $T$ and $\alpha$).

---

## Limits / caveats

- The $O(\alpha^2 T^2)$ matrix-query scaling is **quadratically worse** in $T$ compared to QLSA-based methods, which achieve $O(\alpha T)$. This is the main quantitative drawback. The paper explicitly asks whether this gap can be closed.
- $Q$ can be exponential in $T$ for general dissipative dynamics, just as $\kappa$ can be. The lower bound (Theorem 10) shows you can't do better than linear in $Q$, but for specific problem classes (e.g., normal matrices), $Q$ is well-behaved.
- For non-normal $A$, the amplification ratio $Q$ (defined via the worst-case partition) can vastly overestimate the actual difficulty. The paper gives an explicit $2 \times 2$ example where $Q$ grows exponentially while the solution grows only polynomially. A weaker notion $\bar{Q} = \|\mathcal{T}e^{\int_0^T A(s)\,ds}\| / \||\psi(T)\rangle\|$ would be tighter, but the time-marching algorithm can't achieve $\bar{Q}$-scaling.
- The first-order Magnus integrator's complexity scales as $O(\alpha_{\text{comm}}^2 T^4 Q^3 / \epsilon^2)$ in the high-precision limit — much worse than the Dyson solver when $\alpha_{\text{comm}}$ is large. Useful primarily when commutators are small.
- The compression gadget requires coherent composition of all $L$ block-encodings, meaning the full circuit depth is $\sum_l (\text{depth of } \tilde{U}_l)$ — no parallelism across time steps.

---

## Reusable ideas

1. [[Uniform Singular Value Amplification for Block-Encoding Norm Matching]] — QSVT-based singular value rescaling that adjusts a block-encoding's normalisation to match the actual operator norm, enabling efficient composition of non-unitary operators.

2. [[Compression Gadget for Sequential Block-Encoding Composition]] — coherent binary-tree composition of $L$ block-encodings into a single block-encoding of their product, using $O(\log L)$ extra ancilla qubits and each constituent unitary exactly once.

3. [[Contour Integral Matrix Exponentiation via QSVT]] — implementing $e^A$ for non-normal $A$ (given as a block-encoding) by discretising the Cauchy integral formula and computing each resolvent $(z_k - A)^{-1}$ via QSVT matrix inversion.

---

## References within this paper

- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová, Scherer & Berry (2018)]] — truncated Dyson series for time-dependent Hamiltonians ([47])
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry, Childs, Ostrander & Wang (2017)]] — QLSA-based ODE solver with $\operatorname{polylog}(1/\epsilon)$ precision ([14])
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first QLSA-based ODE solver ([8])
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low & Wiebe (2019)]] — QSVT, the core tool for USVA and matrix inversion ([37, 38])
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2018)]] — interaction-picture simulation; source of the compression gadget ([57])
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes|An, Fang & Lin (2022)]] — qHOP, commutator-scaled Magnus simulation; this paper's first-order Magnus integrator generalises qHOP to non-Hamiltonian case ([4])
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes|Berry, Childs, Su, Wang & Wiebe (2020)]] — L1-norm [[Hamiltonian simulation]] ([7])
- Krovi (2022), *Improved quantum algorithms for linear and nonlinear differential equations*, Quantum **7**, 913 — improved QLSA-based solver with better condition-number scaling ([48])
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim & Lloyd (2009)]] — HHL, the original QLSA ([44])
- Tong, An, Wiebe & Lin (2021), *Fast inversion, preconditioned QLSS, fast Green's-function computation, and fast evaluation of matrix functions*, Phys. Rev. A **104**, 032422 — contour integral method for matrix functions ([61])
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang, Dong & Lin (2022)]] — energy landscape of QSP phase factors ([65]); cited in context of phase-factor computation for QSVT

---

## Cross-links

### Paper notes
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] — truncated Dyson series machinery reused here
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — the main QLSA-based competitor
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — earlier QLSA approach
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT as the core subroutine
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — source of the compression gadget
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]] — Magnus-based [[Hamiltonian simulation]] generalised here
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — HHL, foundational QLSA
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]] — phase-factor computation for QSVT

### Trick cards
- [[Uniform Singular Value Amplification for Block-Encoding Norm Matching]] — new trick card
- [[Compression Gadget for Sequential Block-Encoding Composition]] — new trick card
- [[Contour Integral Matrix Exponentiation via QSVT]] — new trick card
- [[Oblivious Amplitude Amplification (Robust)]] — related amplification technique
- [[Variable-Time Amplification for Condition-Number Reduction]] — related QLSA technique
- [[Standard Amplitude Amplification]] — used for the final success-probability boost
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — QLSA-based alternative achieving $O(T)$ oracle scaling for time-dependent inhomogeneous ODEs; uses same Dyson machinery but different overall architecture
