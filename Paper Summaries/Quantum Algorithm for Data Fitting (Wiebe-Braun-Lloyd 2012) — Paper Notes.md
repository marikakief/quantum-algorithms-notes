> **Source:** Nathan Wiebe, Daniel Braun, Seth Lloyd, *Quantum Algorithm for Data Fitting*, arXiv:1204.5242, Phys. Rev. Lett. **109**, 050505 (2012)
> **Links:** [arXiv](https://arxiv.org/abs/1204.5242) · [PRL](https://doi.org/10.1103/PhysRevLett.109.050505)
> **Tags:** #quantum-ML #linear-algebra #least-squares #HHL #data-fitting #condition-number #quantum-tomography

---

## The computational problem

**Least-squares fitting:** Given $N$ data points $\{(x_i, y_i)\}$ and $M$ basis functions $\{f_j(x)\}$, define the $N \times M$ design matrix $F$ with entries $F_{ij} = f_j(x_i)$ and data vector $\mathbf{y} = (y_1, \ldots, y_N)^\top$. Find the parameter vector $\boldsymbol{\lambda}$ minimising

$$E(\boldsymbol{\lambda}) = \|F\boldsymbol{\lambda} - \mathbf{y}\|^2.$$

The minimiser is the Moore–Penrose pseudoinverse solution $\boldsymbol{\lambda}_{\mathrm{opt}} = F^+ \mathbf{y} = (F^\dagger F)^{-1} F^\dagger \mathbf{y}$ (assuming $F^\dagger F$ invertible).

The paper addresses three sub-problems:
1. **State preparation:** Prepare a quantum state $|\lambda\rangle \propto \boldsymbol{\lambda}_{\mathrm{opt}}$.
2. **Fit-quality estimation:** Estimate $E(\boldsymbol{\lambda}_{\mathrm{opt}})$ without learning $\boldsymbol{\lambda}$ classically.
3. **Classical recovery:** Find a succinct classical description of $\boldsymbol{\lambda}$ when it has low effective dimension $M' \ll M$.

**Complexity measure:** Oracle query complexity to the entries of $F$ (sparse-matrix oracle model); $\tilde{O}$ hides polylogarithmic factors.

---

## What the paper does

Extends HHL from square linear systems to least-squares fitting with a non-square, possibly rank-deficient design matrix. The key construction uses an isometry that embeds $F$ into a Hermitian operator, then applies HHL-style pseudoinverse inversion. Three algorithms are given: one to prepare $|\lambda\rangle$, one to estimate fit quality via a swap test, and one to learn a sparse classical description of $\boldsymbol{\lambda}$ via quantum-assisted compressed sensing.

The result opened the quantum ML algorithms direction — every subsequent quantum ML paper that reduces learning to linear algebra (quantum SVMs, quantum Boltzmann machines, quantum nearest-neighbour) inherits the basic template here.

---

## The algorithm

### Setup: isometry embedding

Because HHL requires a Hermitian matrix, $F$ is embedded via

$$I(F) = \begin{pmatrix} 0 & F \\ F^\dagger & 0 \end{pmatrix} \in \mathbb{C}^{(N+M)\times(N+M)}.$$

Note $I(F)^2 = A := \mathrm{diag}(F^\dagger F,\; F F^\dagger)$, so $A^{-1}$ is block-diagonal and contains $(F^\dagger F)^{-1}$ in the upper block. The pseudoinverse $F^+ = (F^\dagger F)^{-1} F^\dagger$ is therefore implemented as $A^{-1} \cdot I(F^\dagger)$ acting on the input state.

### Algorithm 1 — Prepare $|\lambda\rangle$

**Input:** State $|\mathbf{y}\rangle = \|\mathbf{y}\|^{-1} \sum_{p=M+1}^{M+N} y_p |p\rangle$; condition number bound $\kappa$; sparsity $s$; error $\varepsilon$.

**Output:** $|\lambda\rangle \approx \boldsymbol{\lambda}_{\mathrm{opt}} / \|\boldsymbol{\lambda}_{\mathrm{opt}}\|$ to Euclidean error $\varepsilon$.

**Steps:**

1. **Multiply by $I(F^\dagger)$:** Implement $I(F^\dagger) |\mathbf{y}\rangle$ using [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|phase estimation]]-based eigenvalue multiplication (analogous to HHL but *multiplying* by eigenvalues rather than inverting). Prepare the clock register

   $$|\Psi_0\rangle = \sqrt{\frac{2}{T}}\sum_{\tau=0}^{T-1} \sin\!\left(\frac{\pi(\tau+\tfrac12)}{T}\right) |\tau\rangle \otimes |\mathbf{y}\rangle,$$
   
   apply controlled evolutions $e^{-iI(F^\dagger)\tau t_0/T}$ via sparse Hamiltonian simulation, then QFT and an ancilla-controlled eigenvalue rotation. Postselect on $|1\rangle$; success probability $O(1/\kappa^2)$, boost with $O(\kappa)$ amplitude amplification.

2. **Invert via HHL:** Apply $A^{-1} = I(F)^{-2}$ to the result using HHL on the Hermitian operator $A$. Another round of phase estimation + eigenvalue inversion.

3. **Project:** The combined action of steps 1–2 yields a state proportional to $A^{-1} I(F^\dagger) |\mathbf{y}\rangle = |\lambda\rangle$.

### Algorithm 2 — Estimate fit quality

Prepare two copies of $|\mathbf{y}\rangle$ and $I(F)|\lambda\rangle$ (using Algorithm 1), then run a [[Swap Test for State Comparison|swap test]] to estimate $|\langle \mathbf{y} | I(F) | \lambda\rangle|^2$. Since $E \leq 2(1 - |\langle \mathbf{y} | I(F) | \lambda \rangle|)$ by a Taylor bound, this gives an upper bound on the fit error. Requires $O(1/\delta^2)$ repetitions for error $\delta$.

### Algorithm 3 — Learn a sparse $\boldsymbol{\lambda}$

1. Prepare $|\lambda\rangle$ (Algorithm 1); measure $O(M')$ times to identify the $M'$ largest-weight basis functions.
2. Restrict $F$ to those $M'$ columns; re-prepare $|\lambda\rangle$ for the reduced system.
3. Perform quantum compressed-sensing tomography on the $M'$-dimensional state to reconstruct $\boldsymbol{\lambda}$ classically.
4. Estimate fit quality with Algorithm 2.

---

## Key results

**Theorem (Fit parameter preparation):** For $s$-sparse $F$ with condition number bound $\kappa$, Algorithm 1 prepares $|\lambda\rangle$ to Euclidean error $\varepsilon$ with query complexity

$$\tilde{O}\!\left(\log(N)\, \frac{s^3 \kappa^6}{\varepsilon}\right) \quad \text{(Childs–Kothari simulation)}$$

or alternatively

$$\tilde{O}\!\left(\log(N)\, \frac{s\, \kappa^6}{\varepsilon^2}\right) \quad \text{(LCU-based simulation)}.$$

**Theorem (Fit quality estimation):** Algorithm 2 estimates the fit quality to additive error $\delta$ with query complexity

$$\tilde{O}\!\left(\log(N)\, \frac{s^3 \kappa^4}{\varepsilon\, \delta^2}\right).$$

**Theorem (Classical recovery):** Algorithm 3 recovers a $M'$-sparse classical description of $\boldsymbol{\lambda}$ with query complexity

$$\tilde{O}\!\left(\log(N)\, s^3\!\left(\frac{\kappa^4}{\varepsilon\, \delta^2} + \frac{M'^2 \kappa^6}{\varepsilon^3}\right)\right).$$

**Comparison with classical least-squares:**

| Method | Classical | Quantum (this paper) |
|---|---|---|
| Solving $F\boldsymbol{\lambda} = \mathbf{y}$ | $O(N \cdot M^2)$ direct / $O(NM)$ iterative | $\tilde{O}(\log(N)\, s^3 \kappa^6/\varepsilon)$ |
| Fit quality $E$ | $O(NM)$ residual | $\tilde{O}(\log(N)\, s^3 \kappa^4/(\varepsilon\delta^2))$ |
| Learn sparse $\boldsymbol{\lambda}$ | $O(NM')$ | $\tilde{O}(\log(N)\, s^3 M'^2 \kappa^6/\varepsilon^3)$ |

The quantum speedup in $N$ is exponential — from $O(N)$ to $O(\log N)$ — but only realised under strict conditions (see Caveats).

---

## Comparison with prior work

| Paper | What it does | Relation |
|---|---|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes\|HHL (2009)]] | Solves $Ax = b$ with Hermitian $A$; produces $|x\rangle$ in $\tilde{O}(\log N\, s^2 \kappa^2/\varepsilon)$ | This paper extends to non-square $F$ via isometry; higher $\kappa$ exponents due to two-step inversion |
| [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes\|Childs-Kothari-Somma (2015)]] | Improves $\kappa$ dependence for QLSA | Could improve this paper's bounds if plugged in; later work does so |
| [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes\|Tong-An-Wiebe-Lin (2021)]] | Preconditioned QLSA with better $\kappa$ dependence | Directly subsumes the linear-systems step here |
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes\|HHL note on erratum]] | Original HHL had $s^2$ dependence (erroneous); corrected to $s^4$ | This paper uses improved simulation methods (Childs–Kothari) to get $s^3$ |

---

## Caveats

The quantum speedup is exponential in $N$ but easily killed:

1. **State preparation bottleneck.** Preparing $|\mathbf{y}\rangle$ from classical data is itself $O(N)$ in the worst case (QRAM would help but isn't assumed). The speedup is real only when $|\mathbf{y}\rangle$ is a quantum state already available cheaply — e.g., output of a quantum simulation.

2. **Sparsity requirement.** Dense $F$ (no sparsity structure) makes the simulation cost $O(N)$ and kills the advantage. Works best when $s \ll N$, or when $F$ decomposes into efficiently simulable sparse pieces.

3. **Condition number.** The $\kappa^6$ (or $\kappa^4$) factor is brutal for ill-conditioned systems. Any practical advantage requires $\kappa = O(\mathrm{polylog}\, N)$.

4. **Reading out $\boldsymbol{\lambda}$.** Tomography on the full $M$-dimensional $|\lambda\rangle$ costs $O(M)$ copies, which is $O(N)$ if $M \sim N$ — wiping the advantage. The algorithm is useful only if: (a) one only needs fit quality (Algorithm 2), or (b) $\boldsymbol{\lambda}$ is sparse / low-dimensional ($M' \ll M$).

5. **Invertibility.** Assumes $F^\dagger F$ invertible; ill-conditioning (zero or near-zero eigenvalues) is handled by the pseudoinverse but makes $\kappa$ large.

These caveats are real. The paper was careful to note them. A lot of follow-up quantum ML work was not.

---

## Reusable ideas

1. **Non-square isometry embedding for HHL.** Any rectangular matrix $F$ can be embedded into a Hermitian block operator $I(F)$ without cost, making HHL applicable to non-square systems. See [[Rectangular Matrix Pseudoinverse via Hermitian Isometry Embedding]].

2. **Eigenvalue multiplication via phase estimation.** Phase estimation naturally inverts eigenvalues (HHL); the same circuit implements eigenvalue *multiplication* by relabelling the ancilla rotation. Both operations cost $\tilde{O}(\kappa / \varepsilon)$ applications of the Hamiltonian evolution. See [[Eigenvalue Multiplication via Phase Estimation]].

3. **Fit quality from a swap test.** Estimating $|\langle a | b \rangle|^2$ via the [[Swap Test for State Comparison|swap test]] avoids full state tomography. When $|b\rangle = I(F)|\lambda\rangle$ and $|a\rangle = |\mathbf{y}\rangle$, this directly bounds the residual $E(\boldsymbol{\lambda})$.

4. **Quantum-aided compressed sensing for sparse readout.** When the solution has low effective dimension $M'$, measure the quantum state to identify the support, then restrict and tomograph. Cost $\tilde{O}(M'^2)$ preparations rather than $O(M)$. See [[Quantum-Aided Compressed Sensing for Sparse State Readout]].

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL (2009)]] — foundational QLSA; direct technical predecessor
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari sparse simulation (2010)]] — used for the $s^3$ bounds (this is reference [17] in the paper; the 2012 paper cites the 2010 version of these simulation improvements)
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe LCU (2012)]] — the alternative simulation method giving the $s\kappa^6/\varepsilon^2$ bound; notably this is *the same year*, showing Wiebe was simultaneously developing LCU
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2004)]] — postselected quantum learning framework cited for related state learning
- Gross et al. (2010) — quantum state tomography via compressed sensing; used for the classical readout step

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — the technical foundation; this paper extends HHL to the least-squares setting
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — contemporary work; Wiebe as co-author; the LCU simulation method is the alternative bound in Algorithm 1
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] — later QLSA improvements that improve the $\kappa$ and $s$ dependence here
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — modern QLSA; subsumes the linear-systems subroutine
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — quantum learning via pseudoinverse-style argument; the "predict but don't reconstruct" philosophy echoes here in Algorithm 2
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — later quantum learning paper; both avoid full tomography for efficiency
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT subsumes the eigenvalue manipulation step here; Wiebe is a co-author

### Trick cards
- [[Rectangular Matrix Pseudoinverse via Hermitian Isometry Embedding]]
- [[Eigenvalue Multiplication via Phase Estimation]]
- [[Swap Test for State Comparison]]
- [[Quantum-Aided Compressed Sensing for Sparse State Readout]]
- [[Linear Combination of Unitaries (LCU)]]
