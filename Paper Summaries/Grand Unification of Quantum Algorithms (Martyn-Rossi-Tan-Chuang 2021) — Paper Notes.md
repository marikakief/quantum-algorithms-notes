> **Source:** John M. Martyn, Zane M. Rossi, Andrew K. Tan, and Isaac L. Chuang, *Grand Unification of Quantum Algorithms*, PRX Quantum **2**, 040203 (2021)
> **Links:** [arXiv:2105.02859](https://arxiv.org/abs/2105.02859) · [PRX Quantum](https://doi.org/10.1103/PRXQuantum.2.040203)
> **Tags:** #QSVT #QSP #QET #block-encoding #tutorial #unification #pedagogical

---

## What this paper is

This is a **pedagogical tutorial**, not a paper with new results. If you're looking for the original QSVT theorem, that's [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low & Wiebe (2019)]]. What Martyn et al. do is take the framework and actually explain it — carefully, with worked examples, with circuits drawn out.

The value is entirely educational. They introduce an intermediate object — the **Quantum Eigenvalue Transform (QET)** — that sits between QSP and QSVT and makes the generalization feel natural rather than sudden. They then construct search, phase estimation, Hamiltonian simulation, and matrix inversion from scratch as QSVT instances, showing that these weren't just "also implementable via QSVT" but are in a precise sense the same underlying structure.

If you want to understand *why* QSVT is a unification and not just a restatement, this paper is where to start.

---

## The pedagogical arc

The paper is structured as a three-step generalization:

```
QSP (single qubit, scalar signal a ∈ [-1,1])
   ↓ generalize: embed scalar into block-encoding of Hermitian operator
QET (multi-qubit, eigenvalue transform of Hermitian H)
   ↓ generalize: allow non-Hermitian, apply to singular values
QSVT (multi-qubit, singular value transform of general A)
```

Each step adds exactly one level of generality. This is the paper's actual intellectual contribution: making the QSP → QET → QSVT chain explicit and clean, so the reader can see why block-encoding + projector-controlled phases is the "right" generalization of QSP phases.

See [[QSP-to-QSVT Lifting Chain]] for a compact summary of this progression.

Also see: [[grand-unification-qsp-to-qsvt-hierarchy.excalidraw]] for a visual hierarchy diagram.

---

## Quantum Signal Processing

QSP is the engine. Everything else is built on top of it.

### The single-qubit setup

Signal rotation:
$$W(a) = \begin{pmatrix} a & i\sqrt{1-a^2} \\ i\sqrt{1-a^2} & a \end{pmatrix}, \quad a \in [-1, 1]$$

Processing rotation (phase gate):
$$S(\phi) = \begin{pmatrix} e^{i\phi} & 0 \\ 0 & e^{-i\phi} \end{pmatrix}$$

The QSP sequence of degree $d$ is:
$$U_{\text{QSP}}(a; \vec{\phi}) = S(\phi_0) \prod_{j=1}^{d} \bigl[W(a)\, S(\phi_j)\bigr]$$

The $(0,0)$ entry of this product is a degree-$d$ polynomial in $a$.

### The QSP theorem (what polynomials are realizable)

Given $P, Q \in \mathbb{C}[x]$ with $\deg P \leq d$, $\deg Q \leq d-1$, the polynomial $P(a)$ appears as the top-left entry of $U_{\text{QSP}}(a;\vec\phi)$ if and only if:
1. $P$ has definite parity: same parity as $d$ (even/odd)
2. $|P(a)|^2 + (1 - a^2)|Q(a)|^2 = 1$ for all $a \in [-1,1]$

The key constraint is that $P$ must be norm-bounded on $[-1,1]$ with a specific completion condition. This is why approximation theory is so central — the algorithm design question reduces to: "can I approximate my target function by such a polynomial?"

This is handled in the vault by [[Parity-Aware Polynomial Design]] and [[Polynomial Completion via Sum-of-Squares]].

The [[SU(2) Response Tuple Characterization]] trick captures the algebraic structure that characterizes these polynomials.

---

## From QSP to Quantum Eigenvalue Transform

### Block-encoding as the access model

A block-encoding of $A \in \mathbb{C}^{n \times n}$ is a unitary $U_A$ on a larger space such that:
$$\bigl(\langle\tilde{0}| \otimes I_n\bigr) \, U_A \, \bigl(|\tilde{0}\rangle \otimes I_n\bigr) = \frac{A}{\alpha}$$

where $|\tilde{0}\rangle$ is the ancilla state and $\alpha \geq \|A\|$ is a subnormalization factor. See [[Block-Encoding Composition Algebra]] and [[Standard-Form Encoding (Prepare + Signal Oracle)]] for how these are built and composed.

The block-encoding is the access model that replaces the "signal" $a$ in QSP. When $A$ is Hermitian with eigendecomposition $A = \sum_k \lambda_k |\psi_k\rangle\langle\psi_k|$, the block-encoding contains the eigenvalues $\lambda_k / \alpha$ playing the role of the scalar $a$.

### Projector-controlled phase

The key technical step: replace the single-qubit phase gate $S(\phi) = \text{diag}(e^{i\phi}, e^{-i\phi})$ with a **projector-controlled phase** acting on the ancilla register.

Define $\Pi = |\tilde{0}\rangle\langle\tilde{0}| \otimes I$ and $\Pi^\perp = I - \Pi$. Then:
$$e^{i\phi\Pi} = e^{i\phi}|\tilde{0}\rangle\langle\tilde{0}| \otimes I + |\tilde{0}^\perp\rangle\langle\tilde{0}^\perp| \otimes I$$

This gate applies phase $e^{i\phi}$ to the $|\tilde{0}\rangle$ ancilla subspace and nothing to the complement. It's implementable with controlled-NOT gates and a $Z$ rotation on a single ancilla qubit — see [[Projector-Controlled Phase Gate]] and Martyn et al. Fig. 3.

The Excalidraw circuit diagram is at [[grand-unification-projector-phase-circuit.excalidraw]].

### The QET circuit

For Hermitian $H$ block-encoded in $U_H$, define the QET sequence of degree $d$:
$$U_{\text{QET}}(\vec\phi) = e^{i\phi_0 \Pi} \prod_{j=1}^{d} \bigl[U_H \, e^{i\phi_j \Pi}\bigr]$$

In the invariant 2D subspace spanned by $\{|\tilde{0}\rangle|\psi_k\rangle, |\tilde{0}^\perp\rangle|\psi_k\rangle\}$ for each eigenstate $|\psi_k\rangle$, this acts exactly like the QSP sequence $U_{\text{QSP}}(\lambda_k/\alpha; \vec\phi)$. So the top-left block of $U_{\text{QET}}$ applies $P(\lambda_k/\alpha)$ to each eigenvalue — this is the eigenvalue transform.

This relies on the [[Invariant SU(2) Subspace Embedding]] structure, which is the same machinery as qubitization. The connection to [[Qubitization Iterate]] is direct: qubitization is the special case where the polynomial is chosen to be $e^{-i\lambda t}$ for Hamiltonian simulation.

---

## Quantum Singular Value Transformation

### Extending beyond Hermitian

For a general (possibly non-square) matrix $A$ with singular value decomposition $A = U\Sigma V^\dagger$, the block-encoding is:
$$U_A = \begin{pmatrix} A/\alpha & \cdot \\ \cdot & \cdot \end{pmatrix}$$

QSVT applies a polynomial $P$ to the singular values $\sigma_k / \alpha$:
$$P_{\text{SV}}(A/\alpha) = U \cdot \text{diag}(P(\sigma_1/\alpha), \ldots) \cdot V^\dagger$$

The circuit alternates $U_A$ and $U_A^\dagger$ with projector-controlled phases between them. For odd-degree polynomials:
$$U_{\text{QSVT}}(\vec\phi) = \tilde{\Pi}_{\phi_0} \prod_{j=1}^{(d-1)/2} \bigl[U_A^\dagger \, \Pi_{\phi_{2j-1}} \, U_A \, \tilde\Pi_{\phi_{2j}}\bigr] U_A^\dagger \Pi_{\phi_d}$$

where $\Pi$ and $\tilde\Pi$ act on the left and right ancilla spaces respectively. See [[grand-unification-qsvt-circuit.excalidraw]] for the circuit.

### What this gives you

The design workflow becomes:
1. Block-encode $A$ (determines $\alpha$, the subnormalization)
2. Choose target function $f$ on singular values
3. Approximate $f$ by bounded polynomial $P$ with right parity
4. Compile phase angles $\vec\phi$ (e.g., via QSPPACK or similar)
5. Run $U_{\text{QSVT}}$ — costs $O(\deg P)$ applications of $U_A, U_A^\dagger$

The [[QSVT Meta-Template]] captures this as a reusable recipe.

---

## Block encodings

Block-encodings are the access model that makes QSVT general. The paper treats several types:

| Type | How $A/\alpha$ appears | Notes |
|---|---|---|
| Sparse oracle | sparse matrix from row/column oracles | classical quantum algorithms context |
| LCU / Prepare+Select | $A = \sum_j \alpha_j U_j$, PREPARE + SELECT | $\alpha = \|\vec\alpha\|_1$ |
| Product / composition | $A = BC$, compose block-encodings | $\alpha = \alpha_B \alpha_C$ |
| Tensor product | $A = B \otimes C$ | straightforward |

See [[Block-Encoding Composition Algebra]] for the arithmetic rules. The normalization factor $\alpha$ propagates multiplicatively through compositions and often dominates the final query complexity.

The [[Standard-Form Encoding (Prepare + Signal Oracle)]] trick is the workhorse for chemistry and structured problems.

---

## Applications via QSVT

The paper explicitly constructs each of the following as a QSVT instance. This is the payoff section.

### Search (fixed-point amplitude amplification)

Problem: given oracle $O_f$ marking target state, amplify the target amplitude.

The key polynomial is an approximation to the **sign function** on $[-1,1]$:
$$\text{sign}(a) \approx P_{\text{sign},\kappa}(a)$$
which is $+1$ for $a > \kappa$ and $-1$ for $a < -\kappa$, with transition at threshold $\kappa = 1/\sqrt{N}$ for unstructured search.

This is the [[QSVT Sign-Function Discriminator]] pattern. The sign polynomial is odd-degree, bounded on $[-1,1]$, and approximable to precision $\epsilon$ with degree $O(\kappa^{-1} \log(1/\epsilon))$.

QSVT with this polynomial applied to the oracle gives **fixed-point amplitude amplification** — unlike standard Grover, you don't need to know $\kappa$ exactly, and the algorithm doesn't overshoot. See [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]] and [[Standard Amplitude Amplification]].

### Eigenvalue threshold problem

Problem: given a Hermitian $H$ and threshold $\lambda^*$, decide if $H$ has an eigenvalue below $\lambda^*$.

Same sign-function trick: apply $P_{\text{sign}}((H - \lambda^* I)/\alpha)$ to eigenvalues via QET. Eigenvalues above $\lambda^*$ map to $+1$, eigenvalues below to $-1$. The [[Chebyshev Polynomial Spectral Projector]] technique is closely related — you're effectively doing a spectral step function.

This is a natural stepping stone between search and phase estimation.

### Phase estimation (iterative, semiclassical)

This is one of the more interesting constructions. Standard QPE uses a QFT at the end; here phase estimation falls out of QSVT with no QFT.

Given a unitary $U$ with eigenvector $|\psi\rangle$ and eigenphase $\theta$ (so $U|\psi\rangle = e^{2\pi i\theta}|\psi\rangle$), construct the matrix:
$$A_j(\hat\theta) = \frac{1}{2}\bigl(I + e^{-2\pi i \hat\theta} U^{2^j}\bigr)$$

The singular value of $A_j(\hat\theta)$ applied to $|\psi\rangle$ is $\cos(\pi(\theta - \hat\theta) 2^j)$, which encodes the $j$-th bit of $\theta$ relative to current estimate $\hat\theta$.

Apply the sign-function QSVT to discriminate positive vs. negative — that gives bit $j$ of $\theta$. Update $\hat\theta$ and repeat for the next bit. This is [[Semiclassical Phase Estimation via QSVT]].

No QFT. No quantum control register. The algorithm is iterative and adaptive, similar to [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's]] phase estimation but derived entirely within the QSVT framework.

### Hamiltonian simulation

Given $H$ with $\|H\| \leq 1$, approximate $e^{-iHt}$.

The target function on eigenvalues is $f(\lambda) = e^{-i\lambda t}$. This is a smooth, bounded function on $[-1,1]$, approximable by a polynomial of degree $O(t + \log(1/\epsilon)/\log\log(1/\epsilon))$ via [[Jacobi-Anger Truncation for QSP Simulation]].

QSVT applies this polynomial to the block-encoded $H$, giving the simulation. The complexity matches the [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang]] result, which is optimal.

The [[Phased Qubitization Sequence]] is the specialized form this takes for qubitized Hamiltonians.

### Matrix inversion

Given $A$ with singular values bounded away from zero ($\sigma_{\min} \geq 1/\kappa$), apply $f(\sigma) = 1/\sigma$.

The function $1/x$ diverges at $x = 0$, so you need a truncated inverse. Approximate $1/x$ on $[\kappa^{-1}, 1]$ with precision $\epsilon$ by a polynomial of degree $O(\kappa \log(1/\epsilon))$ — this is [[Chebyshev-Regularized Inverse Approximation]].

The QSVT circuit applies this to the singular values of $A$, effectively implementing $A^{-1}$ (up to normalization). This subsumes the HHL algorithm — see [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]].

---

## The unification argument

The paper's title claim is: search, phase estimation, Hamiltonian simulation, and matrix inversion are all instances of QSVT with different polynomial choices.

| Algorithm | Target function | Polynomial degree | Access oracle |
|---|---|---|---|
| Search (FP-AA) | $\text{sign}(a)$ at $\kappa = 1/\sqrt{N}$ | $O(\kappa^{-1})$ | Grover diffusion oracle |
| Eigenvalue threshold | $\text{sign}((\lambda - \lambda^*)/\alpha)$ | $O(\delta^{-1} \log(1/\epsilon))$ | Block-encoding of $H$ |
| Phase estimation (1 bit) | $\text{sign}(\cos(\pi \cdot))$ | $O(\epsilon^{-1})$ | Controlled-$U^{2^j}$ |
| Hamiltonian simulation | $e^{-i\lambda t}$ | $O(t + \log(1/\epsilon))$ | Block-encoding of $H$ |
| Matrix inversion | $1/\sigma$ on $[1/\kappa, 1]$ | $O(\kappa \log(1/\epsilon))$ | Block-encoding of $A$ |

The unification is not just taxonomic — it means that algorithmic improvements to the polynomial approximation machinery (better degree bounds, better compilation of phase angles) propagate automatically to all of these algorithms simultaneously.

See [[grand-unification-algorithm-unification.excalidraw]] and [[QSP-to-QSVT Lifting Chain]].

---

## Figures and diagrams

Key figures in the paper:
- **Fig. 1:** The QSP circuit — $d$ alternating $W(a)$ and $S(\phi_j)$ gates
- **Fig. 2:** The QET circuit showing how $U_A$ replaces $W(a)$ and projector-controlled phases replace $S(\phi_j)$
- **Fig. 3:** The projector-controlled phase gate construction via ancilla + controlled-NOTs + $Z$ rotation
- **Fig. 4:** Polynomial approximations to sign, $e^{-ixt}$, and $1/x$ on $[-1,1]$
- **Fig. 5:** The QSVT circuit showing alternating $U_A$, $U_A^\dagger$ with left/right projector phases
- **Fig. 6:** The grand unification diagram showing algorithms as points in polynomial-choice space

Vault diagrams:
- [[grand-unification-qsp-to-qsvt-hierarchy.excalidraw]] — hierarchy of QSP → QET → QSVT
- [[grand-unification-projector-phase-circuit.excalidraw]] — the projector-controlled phase construction
- [[grand-unification-qsvt-circuit.excalidraw]] — the full QSVT circuit
- [[grand-unification-algorithm-unification.excalidraw]] — algorithms as QSVT instances

---

## Key results

**Theorem (QSP Achievability):** For any $P \in \mathbb{C}[x]$ of degree $d$ with definite parity and $|P(a)|^2 + (1-a^2)|Q(a)|^2 = 1$ for all $a \in [-1,1]$ (for some $Q$ of appropriate degree), there exist phases $\phi_0, \ldots, \phi_d \in \mathbb{R}$ such that $[U_{\text{QSP}}(a;\vec\phi)]_{00} = P(a)$.

**Theorem (QET):** Let $U_H$ be a block-encoding of Hermitian $H/\alpha$ on ancilla $|\tilde{0}\rangle$, and let $P$ satisfy the QSP achievability conditions. Then the QET circuit of depth $d$ implements:
$$\bigl(\langle\tilde{0}|\otimes I\bigr) U_{\text{QET}}(\vec\phi) \bigl(|\tilde{0}\rangle \otimes I\bigr) = P(H/\alpha)$$

**Theorem (QSVT):** For $P$ a degree-$d$ polynomial satisfying the QSP conditions, and $U_A$ a block-encoding of $A/\alpha$, the QSVT circuit applies $P$ to the singular values:
$$\bigl(\langle\tilde{0}|\otimes I\bigr) U_{\text{QSVT}}(\vec\phi) \bigl(|\tilde{0}\rangle\otimes I\bigr) = P^{(\text{SV})}(A/\alpha)$$

These are all from [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|GSLW 2019]]; Martyn et al. present them with cleaner proof sketches and circuit intuition.

---

## Limits / caveats

This is a tutorial, and it explicitly omits or defers several important practical issues:

**Phase angle computation.** The paper states existence of phase angles but gives no algorithm to find them. In practice you need numerical methods (QSPPACK, pyqsp, etc.) which are non-trivial for high-degree polynomials and can be numerically unstable. This is an active research area.

**Practical compilation.** The circuits assume exact gates. Physical gate compilation, fault tolerance, and T-gate counting are not discussed.

**Noise.** Everything is ideal. No discussion of how QSVT degrades under errors, or how to choose polynomial degree vs. circuit noise tradeoffs.

**Block-encoding costs.** The paper mostly assumes a block-encoding is given. For real problems, constructing the block-encoding can cost more than the QSVT part — especially for chemistry Hamiltonians.

**Non-Hermitian extensions.** Standard QSVT handles singular values of general matrices, but non-normal eigenvalue processing needs more care — see [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]].

**Continuous-time / multi-variable.** Single-variable polynomials. Multi-variable QSP and other generalisations are not covered.

---

## Reusable ideas

Trick cards directly relevant to or introduced by this paper:

- [[Projector-Controlled Phase Gate]] — the key building block for lifting QSP to QET/QSVT
- [[QSP-to-QSVT Lifting Chain]] — the conceptual chain QSP → QET → QSVT
- [[QSVT Sign-Function Discriminator]] — sign polynomial for search, eigenvalue threshold, phase estimation
- [[Semiclassical Phase Estimation via QSVT]] — bit-by-bit phase extraction via sign-function QSVT

Existing trick cards also used:
- [[QSVT Meta-Template]] — master recipe
- [[Parity-Aware Polynomial Design]] — constraints on realizable polynomials
- [[One-Ancilla Alternating Phase Sequence]] — the alternating phase/block sequence
- [[Invariant SU(2) Subspace Embedding]] — why QET works on each eigenspace
- [[Jacobi-Anger Truncation for QSP Simulation]] — polynomial degree for Hamiltonian simulation
- [[Standard Amplitude Amplification]] — subsumed as QSVT instance
- [[Oblivious Amplitude Amplification (Robust)]] — postselection recovery
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]] — fixed-point search
- [[Chebyshev Polynomial Spectral Projector]] — eigenvalue threshold
- [[Chebyshev-Regularized Inverse Approximation]] — matrix inversion
- [[Block-Encoding Composition Algebra]] — building block-encodings
- [[Standard-Form Encoding (Prepare + Signal Oracle)]] — standard access model
- [[Phased Qubitization Sequence]] — specialized circuit for qubitized simulation
- [[SU(2) Response Tuple Characterization]] — algebraic characterization of QSP polynomials
- [[Polynomial Completion via Sum-of-Squares]] — finding completion Q given P
- [[FIR-Style Minimax Design for Quantum Responses]] — approximation theory for QSP
- [[Linear Combination of Unitaries (LCU)]] — one standard block-encoding method

---

## References within this paper

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low & Wiebe (2019)]] — the foundational QSVT paper; all theorems here are from that work
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — original QSP for Hamiltonian simulation
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization framework; QSVT generalizes this
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low, Yoder & Chuang (2016)]] — composite pulse sequences, QSP precursors
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — search algorithm, shown as QSVT instance
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation; related to the semiclassical QPE construction
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — amplitude amplification subsumed by QSVT
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim & Lloyd (2009)]] — HHL matrix inversion, shown as QSVT instance
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin & Tong (2020)]] — eigenstate filtering via QSVT

---

## Cross-links

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Projector-Controlled Phase Gate]]
- [[QSP-to-QSVT Lifting Chain]]
- [[QSVT Sign-Function Discriminator]]
- [[Semiclassical Phase Estimation via QSVT]]
- [[QSVT Meta-Template]]
- [[Block-Encoding Composition Algebra]]
- [[Standard-Form Encoding (Prepare + Signal Oracle)]]
- [[Parity-Aware Polynomial Design]]
- [[Invariant SU(2) Subspace Embedding]]
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] — same first+last authors; applies the QET framework from this paper to build fully-coherent simulation, including the one-shot parity-bypass construction
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]] — same first author; improves the QSP/QSVT family from this paper by a factor of 2 on query complexity via stochastic polynomial ensembles and the mixing lemma
