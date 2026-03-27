> **Paper:** Quantum eigenvalue processing
> **arXiv:** [2401.06240](https://arxiv.org/abs/2401.06240)
> **Published:** FOCS 2024 (pp. 1051–1062); SIAM Journal on Computing **55**(1), 135–215 (2026)
> **Date read:** 2026-03-13
> **Tags:** #qevt #qeve #non-normal #block-encoding #qlsa #qsvt-adjacent

---

## One-Line Take

QSVT processes singular values — fine for Hermitian matrices where $\sigma(A) = |\lambda(A)|$, but wrong for non-normal matrices where eigenvalues and singular values decouple. This paper builds the missing framework: polynomial processing of eigenvalues directly, for block-encoded non-normal operators.

---

## The Problem QSVT Doesn't Solve

QSVT applies polynomial $p$ to the singular values of a block-encoded matrix $A$, producing $p(A^\dagger A)^{1/2}$ (roughly). For Hermitian $A$, singular values $= |\text{eigenvalues}|$, so QSVT gives access to $|p(\lambda)|$-type functions of eigenvalues. But for non-normal $A$ — arising naturally in non-Hermitian physics, ODEs with non-symmetric coefficient matrices, or linear systems where the iteration matrix is non-normal — singular values can be far from $|\lambda|$, and QSVT gives the wrong thing.

The paper introduces two primitives:
- **QEVT** (Quantum EigenValue Transformation): apply polynomial $f$ to the eigenvalues of $A$, producing (approximately) $f(A)$ in block-encoded form.
- **QEVE** (Quantum EigenValue Estimation): estimate eigenvalues of $A$ to precision $\varepsilon$ with Heisenberg-limited $O(1/\varepsilon)$ query complexity, for diagonalizable matrices with real spectra.

---

## Core Technical Machinery

### 1. Polynomial Basis History States

The key construction is the **polynomial basis history state**. For a polynomial basis $\{p_k\}$ (e.g., Chebyshev or Faber), and a block-encoded matrix $A$ with eigenvectors $\{|\psi_j\rangle\}$ and eigenvalues $\{\lambda_j\}$, the state:

$$|\Phi\rangle = \sum_{k=0}^{d-1} c_k |k\rangle |p_k(A)|\psi_j\rangle\rangle$$

encodes a degree-$(d-1)$ polynomial of $A$ applied to $|\psi_j\rangle$, in superposition over the basis index $k$. Since $p_k(\lambda_j)$ is the coefficient of the target polynomial evaluated at the eigenvalue, measuring in the right basis extracts $\lambda_j$.

**Key point:** Building this state naively costs $O(d)$ block-encoding queries (one per polynomial degree). The paper avoids this.

### 2. Generating-Function-to-QLSA Reduction

Instead of building the history state term by term, note that the sequence $\{p_k(A)|v\rangle\}$ satisfies the Chebyshev (or Faber) recurrence:
$$p_{k+1}(A)|v\rangle = 2A\,p_k(A)|v\rangle - p_{k-1}(A)|v\rangle$$

This recurrence has the form of a block-tridiagonal linear system on the register $|k\rangle \otimes \mathcal{H}_\text{system}$. Inverting this system — via a QLSA call — prepares the full superposition $\sum_k |k\rangle |p_k(A)|v\rangle$ using $O(\text{polylog}(d))$ block-encoding queries (hidden in the QLSA condition number), rather than $O(d)$.

The structure encoded is essentially the lower-shift operator on the $k$-register combined with $A$ on the system, yielding a banded linear system of size $d \times \dim(\mathcal{H})$.

### 3. Lower-Shift Ancilla Encoding

The lower-shift operator $L$ on the $|k\rangle$ register acts as $L|k\rangle = |k{-}1\rangle$ (zero on $|0\rangle$). The Chebyshev recurrence is captured by the block system:

$$(I \otimes L^\dagger - 2A \otimes I + I \otimes L)|p\rangle = |b\rangle$$

where $|p\rangle = \sum_k |k\rangle|p_k(A)v\rangle$ and $|b\rangle$ encodes boundary conditions. The shift structure makes the system banded (bandwidth 1 in the $k$ register), enabling efficient QLSA application. The ancilla register holding $k$ acts purely algebraically — no time evolution of $A$ is needed.

### 4. Chebyshev-State Phase Estimation (QEVE)

For diagonalizable real-spectrum $A$ with eigenvalues $\lambda_j \in [-1,1]$, write $\lambda_j = \cos\theta_j$. The Chebyshev basis satisfies $T_k(\lambda_j) = \cos(k\theta_j)$, so the history state becomes:

$$|\Phi_j\rangle = \sum_{k=0}^{d-1} c_k |k\rangle \cos(k\theta_j) |\psi_j\rangle$$

This is a discrete Fourier series in $k$ with frequency $\theta_j$. Applying a QFT on the $k$-register followed by phase estimation gives $\theta_j$ to precision $1/d$ (Heisenberg-limited: $d = O(1/\varepsilon)$ Chebyshev terms). Compared to QPE on $e^{-iAt}$ (which requires $t = O(1/\varepsilon)$ simulation time), this achieves the same precision but via QLSA inversion rather than long simulation.

**Caveat:** The success probability depends on the eigenbasis condition number $\kappa_V$ (where $A = V\Lambda V^{-1}$). Cost scales with $\kappa_V$.

### 5. Faber Polynomials for Complex Spectra (QEVT)

Chebyshev works for real intervals. For non-normal $A$ with complex spectrum, the natural generalization is **Faber polynomials** for a region $E$ containing the spectrum.

Given a conformal map $\phi: \mathbb{C} \setminus E \to \mathbb{C} \setminus \mathbb{D}$ (exterior of $E$ to exterior of unit disk), the Faber polynomials $\{F_n\}$ are defined by the Laurent expansion of $\phi$:
$$[\phi(z)]^n = F_n(z) + \text{(terms analytic inside }E\text{)}$$

They satisfy: $\sup_{z \in E} |F_n(z)| = O(1)$ (bounded on $E$), and they provide near-best polynomial approximations on $E$ (analogous to Chebyshev on $[-1,1]$). The QEVT uses Faber polynomials as the basis, with the conformal map data supplied classically.

**Efficient Faber state preparation:** The paper develops an efficient method to prepare the quantum superposition $\sum_n a_n |n\rangle |F_n(A)|v\rangle$ in $O(\text{polylog}(d))$ block-encoding queries, which the paper calls "of independent interest."

### 6. Fourier Coefficient State Preparation in Polylog Time

A persistent bottleneck in polynomial-based quantum algorithms is loading polynomial coefficients into a quantum register. Naively, loading $n$ Chebyshev/Faber coefficients into a superposition costs $O(n)$ gates (linear state preparation).

The paper shows these coefficients have Fourier structure: $c_k = (2/\pi)\int_0^\pi f(\cos\theta)\cos(k\theta)\,d\theta$ (a DCT). Applying a QFT to the coefficient index register generates the superposition in $O(\text{polylog}(n))$ gates — an exponential improvement that has uses beyond this paper.

---

## Complexity Summary

| Algorithm | Input | Complexity |
|-----------|-------|------------|
| QEVE (eigenvalue estimation) | Diagonalizable, real spectrum, $\kappa_V$ eigenbasis condition | $O(\kappa_V / \varepsilon)$ block-encoding queries — Heisenberg-limited in $\varepsilon$ |
| QEVT (eigenvalue transform) | Non-normal, complex spectrum, region $E$ | Query complexity nearly matching QSVT for Hermitian case |
| Linear DE solver | $\dot{u} = Au + b$, average-case diagonalizable | **Strictly linear time:** $O(\kappa_V \cdot t / \varepsilon)$ — vs $O(\kappa_V^2 t / \varepsilon)$ or worse for prior methods |
| Ground state prep | Diagonalizable, real spectrum | Near-optimal (upgrades prior Hermitian results) |
| Fourier coefficient load | $n$ coefficients | $O(\text{polylog}(n))$ gates |

---

## Key Departure from QSVT

QSVT guarantees: $\langle 0|U|0\rangle = p(A^\dagger A)^{1/2} / \|p\|$ — a polynomial in singular values, regardless of spectral structure.

QEVT guarantees: $\langle 0|U|0\rangle \approx f(A) / \|f\|$ for polynomial $f$ — a polynomial in **eigenvalues**, meaningful for diagonalizable $A$.

The two coincide for Hermitian $A$ (where $A = A^\dagger$ and singular values $= |\lambda|$). The non-Hermitian case requires the history-state / QLSA approach.

---

## Caveats

- The eigenbasis condition number $\kappa_V$ can be exponentially large for highly non-normal matrices. The paper tables several non-normality measures (Jordan condition number, numerical range, pseudospectrum) and their effect on cost — useful for assessing practical applicability.
- QLSA subroutines require the linear system to be well-conditioned; the banded structure helps but doesn't eliminate this.
- Faber polynomial computation requires reliable conformal map data for the spectral region $E$. In practice this is classical preprocessing.
- "Strictly linear" time for DE solver is an average-case result for diagonalizable operators; worst-case non-normal operators (near-Jordan blocks) still incur heavy penalties.

---

## Reusable Tricks

- [[Generating-Function-to-QLSA Reduction for Polynomial Basis Histories]]
- [[Lower-Shift Ancilla Encoding for Polynomial Degree Management]]
- [[Chebyshev-State Phase Estimation for Real-Spectrum Non-Normal Inputs]]
- [[Faber-Polynomial Region Mapping for Complex-Spectrum Eigenvalue Transforms]]
- [[Fourier-Convolution Coefficient-State Preparation in Polylog Time]]

---

## References within this paper

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — [[QSVT Meta-Template|QSVT]] for normal/Hermitian matrices; this paper extends to non-normal
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization (Hermitian case)
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs, Kothari & Somma (2017)]] — Chebyshev/Fourier approaches to matrix functions
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry, Childs, Ostrander & Wang (2017)]] — linear ODE algorithms that motivate non-normal eigenvalue transforms
- Trefethen & Embree (2005) — pseudospectra theory (classical background for non-normal analysis)

---

## Cross-References

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]] — same authors (An, Childs, Lin); alternative approach to non-normal eigenvalue transforms via Laplace/LCHS rather than history states
- [[Hamiltonian Simulation — Comparison Tables]]
