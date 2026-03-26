> **Source:** Yu Tong, Dong An, Nathan Wiebe, and Lin Lin, *Fast inversion, preconditioned quantum linear system solvers, fast Green's-function computation, and fast evaluation of matrix functions*, arXiv:2008.13295, Phys. Rev. A **104**, 032422 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2008.13295) · [Phys. Rev. A](https://doi.org/10.1103/PhysRevA.104.032422)
> **Tags:** #QLSA #preconditioning #block-encoding #QSVT #Greens-function #Gibbs-state #matrix-function #LCU

---

## The computational problem

**Quantum linear system problem (QLSP):** Given block-encodings of $A$ and $B$, and a state $|b\rangle$, prepare a state proportional to $(A + B)^{-1}|b\rangle$.

The twist: $A$ is large (high norm, drives up condition number) but has classically-accessible eigenvalues, so its inverse can be constructed cheaply. $B$ is the "hard" part but has smaller norm.

**Applications:** single-particle Green's functions $G_{ij}(z) = \langle 0| a_i (z - H + E_0)^{-1} a_j^\dagger |0\rangle$, and matrix functions like $e^{-\beta H}$ for Gibbs state preparation.

---

## What the paper does

Introduces a quantum primitive called **fast inversion** that block-encodes $A^{-1}$ at cost independent of $\kappa(A)$ when $A$ has classically-computable eigenvalues (diagonal, normal via FFT, etc.). Uses this as a preconditioner: instead of inverting $(A+B)$ directly, solve $(I + A^{-1}B)|x\rangle \propto A^{-1}|b\rangle$ via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]. The cost depends on $\alpha_B$ and the smallest singular value of the preconditioned operator, not on $\|A\|$.

This is the classical preconditioning idea — widely used in iterative solvers — ported to the quantum setting with a clean block-encoding implementation.

---

## The algorithm / construction

### Fast inversion primitive

For a diagonal matrix $D$ with oracle access to its entries:

1. Query the diagonal entry $d_j$ into an ancilla register
2. Compute $1/d_j$ via reversible classical arithmetic
3. Perform a controlled rotation: $|0\rangle \to \frac{1}{\alpha'_A d_j}|0\rangle + \sqrt{1 - \frac{1}{(\alpha'_A d_j)^2}}|1\rangle$
4. Uncompute the arithmetic
5. Postselect on $|0\rangle$

This gives a $(\alpha'_A, m, 0)$-[[Block-Encoding Composition Algebra|block-encoding]] of $D^{-1}$ where $\alpha'_A = \|D^{-1}\|$. The circuit uses $O(1)$ oracle queries — independent of $\kappa(D)$.

For **normal matrices** $A = VDV^\dagger$ where $V$ is efficiently implementable (e.g., QFT, [[Basis Switching via QFT for Kinetic-Potential Splitting|FFFT]]) and eigenvalues are classically computable: apply $V^\dagger$, fast-invert $D$, apply $V$. One use each of $V$ and $V^\dagger$.

### Preconditioned linear system solver

Given:
- A fast-invertible $A$ with $(\alpha'_A, m_A, 0)$-block-encoding of $A^{-1}$
- An $(\alpha_B, m_B, \epsilon_B)$-block-encoding of $B$

Construct the preconditioned operator $W = I + A^{-1}B$ via [[Block-Encoding Composition Algebra|block-encoding multiplication and addition]]. Then:

1. Build a block-encoding of $W$ with normalisation $\alpha_W = 1 + \alpha'_A \alpha_B$
2. Apply [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] to implement $W^{-1}$ as a polynomial of degree $O(\alpha_W / \tilde{\sigma}_{\min} \cdot \log(1/\epsilon))$, where $\tilde{\sigma}_{\min}$ lower-bounds the smallest singular value of $W$
3. Multiply by the block-encoding of $A^{-1}$ to get $(A+B)^{-1}$
4. Apply to $|b\rangle$ and use [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]] to boost success, at cost $O(1/\xi)$ where $\xi = \|(A+B)^{-1}|b\rangle\|$

### Green's function computation

For a many-body Hamiltonian $H$ with ground state $|0\rangle$ at energy $E_0$:

$$G_{ij}(z) = \langle 0| a_i (z - H + E_0)^{-1} a_j^\dagger |0\rangle$$

Partition $H = T + V$ where $T$ (kinetic) is fast-invertible:
- **Hubbard model:** $T$ is diagonalised by the fermionic fast Fourier transform (FFFT)
- **Plane-wave dual:** $T$ is diagonal in the plane-wave basis
- **Schwinger model:** free-fermion hopping is diagonalised by standard QFT

Set $A = z - T + E_0$ (fast-invertible) and $B = -V$ (interaction, smaller norm). Extract $G_{ij}(z)$ via the nonunitary Hadamard test on the preconditioned inverse.

### Matrix functions and Gibbs state preparation

Two routes to implement $e^{-\beta H}$:

**Route 1: Contour integral + LCU.** Write
$$e^{-\beta H} = \frac{1}{2\pi i} \oint_\Gamma e^{-\beta z}(z - H)^{-1}\,dz$$
Discretise with $K$ quadrature points on a parabolic contour. Each resolvent $(z_k - H)^{-1}$ is computed via the preconditioned solver. Combine by [[Linear Combination of Unitaries (LCU)|LCU]] with coefficients from the quadrature weights. Block-encoding normalisation: $\alpha_{\text{LCU}} = O(\alpha_B\sqrt{\beta})$.

This is related to but distinct from the [[Contour Integral Matrix Exponentiation via QSVT]] trick: here the resolvents use preconditioned inversion rather than direct QSVT inversion, which is the advantage when $H = A + B$ with $A$ fast-invertible.

**Route 2: Inverse transform + QSVT.** Write $f(x) = e^{-\beta x}$ and note that if $A$ is fast-invertible, then $f(A + B) = g(W)$ where $g$ is a function of the preconditioned variable. Approximate $g$ by a Chebyshev polynomial and implement via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]. The paper analyses the Gevrey-class smoothness of $g$ to bound the polynomial degree.

**Gibbs state:** Prepare the purified Gibbs state $|\Phi_\beta\rangle \propto (e^{-\beta H/2} \otimes I)|\Omega\rangle$ where $|\Omega\rangle$ is maximally entangled.

---

## Key results

**Theorem 1 (Preconditioned block-encoding of inverse):** Given a fast-invertible $A$ and block-encoding of $B$, one can construct a block-encoding of $(A+B)^{-1}$ using
$$O\!\left(\frac{\alpha'_A \alpha_B}{\tilde{\sigma}_{\min}} \log\frac{1}{\epsilon}\right)$$
queries to the block-encodings, where $\tilde{\sigma}_{\min}$ lower-bounds the smallest singular value of $I + A^{-1}B$.

**Corollary 1 (Preconditioned QLSP):** Prepares $(A+B)^{-1}|b\rangle$ to error $\epsilon$ using
$$O\!\left(\frac{1}{\xi} \cdot \frac{\alpha'_A \alpha_B}{\tilde{\sigma}_{\min}} \log\frac{1}{\epsilon}\right)$$
queries, where $\xi = \|(A+B)^{-1}|b\rangle\|$. The state-dependent parameter $\xi$ replaces worst-case $\kappa$ dependence.

**Lower bound (Remark 5):** The $1/\xi$ dependence is tight — follows from a reduction to unstructured search.

**Theorem 2 (Green's function):** Approximates $G_{ij}(z)$ to additive error $\epsilon$ with query complexity depending on $\alpha_V$ (interaction norm), $\tilde{\sigma}_{\min}$, and ground-state preparation cost — not on $\|T\|$ (kinetic norm).

**Theorems 3–6 (Matrix functions):**
- Contour approach: block-encoding normalisation $\alpha_{\text{LCU}} = O(\alpha_B\sqrt{\beta})$
- Inverse-transform approach: polynomial degree bounded via Gevrey-class analysis (Faà di Bruno formula for composed function derivatives)

---

## Comparison with prior work

| Method | $\kappa$-dependence | Precision | Notes |
|---|---|---|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes\|HHL (2009)]] | $O(\kappa^2)$ | $O(\text{poly}(1/\epsilon))$ | Phase estimation bottleneck |
| [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes\|Childs-Kothari-Somma (2017)]] | $O(\kappa)$ | $O(\text{polylog}(1/\epsilon))$ | LCU-based |
| Direct QSVT inversion | $O(\kappa)$ | $O(\log(1/\epsilon))$ | Via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes\|QSVT]] |
| **This paper** | $O(\alpha'_A \alpha_B / (\xi \tilde{\sigma}_{\min}))$ | $O(\log(1/\epsilon))$ | Can be $\ll \kappa$ when $A$ dominates |

The advantage is structural: when $H = T + V$ with $T$ large but fast-invertible and $V$ small, the preconditioned cost depends on $\alpha_V$ rather than $\|T\| + \|V\|$.

---

## Limits / caveats

- **Fast-invertibility is a strong assumption.** The matrix $A$ must be diagonalisable by an efficiently-implementable unitary with classically-computable eigenvalues. This fits kinetic operators in standard bases but not arbitrary Hamiltonians.
- **$\tilde{\sigma}_{\min}$ can be small.** If $A^{-1}B$ has eigenvalues near $-1$, the preconditioned system is ill-conditioned and the advantage evaporates. The bound $\tilde{\sigma}_{\min} \geq \eta/(\eta + \alpha_B + 1)$ (where $\eta$ is the spectral gap of $H$ above $E_0$) shows this tracks the physical gap.
- **State-dependent $\xi$ is a double-edged sword.** It can be much better than $1/\kappa$ for well-conditioned right-hand sides, but in the worst case $\xi \sim 1/\kappa$ and you recover standard scaling.
- **Ground state preparation cost.** For the Green's function application, you still need to prepare $|0\rangle$. This cost is separate and can dominate.
- **Contour integral approach for $e^{-\beta H}$** requires $O(\alpha_B\sqrt{\beta})$ normalisation, which grows with inverse temperature. At low temperature the LCU coefficients blow up.

---

## Reusable ideas

1. [[Fast Inversion for Classically-Diagonalizable Matrices]] — the primitive that block-encodes $A^{-1}$ at cost independent of $\kappa(A)$ when the eigenvalue structure is classically accessible
2. [[Preconditioning Quantum Linear Systems via Fast Inversion]] — using $A^{-1}$ as a preconditioner to shift the cost from $\|A\|$ to $\|B\|$; the quantum analogue of classical preconditioning
3. [[Inverse-Transform QSVT for Matrix Functions]] — implementing $f(A+B) = g((A+B)^{-1})$ by Chebyshev-approximating $g$ and applying QSVT to the preconditioned inverse; Gevrey-class smoothness analysis for degree bounds

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — the original QLSP algorithm; this paper improves its $\kappa$-dependence via preconditioning
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — QSVT is the main tool for polynomial transforms of block-encoded operators
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2017)]] — prior QLSP with polylog precision via [[Fourier Integral LCU for Matrix Inversion|Fourier/Chebyshev LCU]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]] — interaction-picture simulation also exploits $H = A + B$ splitting; related philosophy but different technique
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams-Lloyd (1997)]] — early fermionic simulation; Hubbard model context
- Clader, Jacobs & Sprouse, *Preconditioned quantum linear system algorithm*, PRL 2013 (arXiv:1301.2340) — earlier preconditioned QLSP using HHL-era techniques; this paper supersedes it with block-encoding + QSVT
- Trefethen & Weideman (2014) — exponentially convergent trapezoidal rule; provides the quadrature error bounds for the contour integral approach

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]]
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — concurrent QLSP work achieving optimal $O(\kappa\log(1/\varepsilon))$ via discrete adiabatic theorem; complementary approach to the preconditioning strategy here
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — argues that QLSP-based ODE approaches (including preconditioned variants) introduce condition-number overhead that prevents exponential speedup for oscillator dynamics

### Trick cards
- [[Fast Inversion for Classically-Diagonalizable Matrices]]
- [[Preconditioning Quantum Linear Systems via Fast Inversion]]
- [[Inverse-Transform QSVT for Matrix Functions]]
- [[Block-Encoding Composition Algebra]]
- [[Fourier Integral LCU for Matrix Inversion]]
- [[Contour Integral Matrix Exponentiation via QSVT]]
- [[Basis Switching via QFT for Kinetic-Potential Splitting]]
- [[Oblivious Amplitude Amplification (Robust)]]
