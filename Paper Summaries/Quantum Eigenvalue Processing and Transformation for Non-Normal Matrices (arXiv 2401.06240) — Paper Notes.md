> **Source:** Guang Hao Low and Yuan Su, *Quantum eigenvalue processing*, FOCS 2024, pp. 1051–1062; SIAM J. Comput. **55**(1), 135–215 (2026)
> **Links:** [arXiv](https://arxiv.org/abs/2401.06240) · [FOCS](https://doi.org/10.1109/FOCS61266.2024.00070) · [SIAM](https://doi.org/10.1137/24M1689363)
> **Tags:** #qevt #qeve #non-normal #block-encoding #qsvt-adjacent #linear-systems #matrix-functions

---

## The computational problem

Given a block-encoding of a diagonalizable but generally **non-normal** matrix $A$, implement a polynomial transformation $f(A)$ of its **eigenvalues**, or estimate those eigenvalues directly. The obstacle is that [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] processes singular values, not eigenvalues. For Hermitian inputs that distinction collapses; for non-normal matrices it does not.

So the formal problem is: starting from a block-encoding of $A$, produce a block-encoding of $f(A)$ with complexity close to the Hermitian/QSVT case, or estimate eigenvalues of $A$ to precision $\varepsilon$ with Heisenberg scaling when the spectrum is real.

This matters for non-Hermitian physics, linear differential equations, non-normal iteration matrices, and matrix-function problems where the relevant object is $f(\lambda)$ rather than $f(\sigma)$.

## What the paper does

This paper builds the missing non-normal analogue of the [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] / [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] toolkit. It introduces **Quantum EigenValue Transformation** (QEVT), which applies polynomial transformations to eigenvalues of block-encoded non-normal operators, and **Quantum EigenValue Estimation** (QEVE), which estimates eigenvalues of diagonalizable real-spectrum matrices at Heisenberg-limited precision.

The technical engine is a history-state construction in a polynomial basis. Rather than implementing a numerically unstable monomial expansion or treating each polynomial component as an unrelated subroutine, the paper converts the polynomial recurrence into a structured linear system and solves that system quantumly. This manages the polynomial basis coherently, but it does not make arbitrary high-degree transformations cost only polylogarithmically in the degree; target degree/precision, non-normality, and linear-system conditioning still enter the complexity. For real spectra the basis is Chebyshev; for complex spectra it is Faber. That is the conceptual jump: instead of encoding the matrix through a two-level walk geometry as in qubitization, encode the whole polynomial basis through a recurrence-resolvent viewpoint.

## The algorithm / construction

### 1. Polynomial-basis history states

Let $\{p_k\}_{k=0}^{d-1}$ be a polynomial basis. For an eigenpair $A|\psi_j\rangle = \lambda_j |\psi_j\rangle$, the ideal history state has the form

$$
|\Phi_j\rangle = \sum_{k=0}^{d-1} c_k |k\rangle \, p_k(A)|\psi_j\rangle
= \sum_{k=0}^{d-1} c_k p_k(\lambda_j) |k\rangle |\psi_j\rangle.
$$

If this state can be prepared efficiently, then measuring the $k$ register in the right basis implements either eigenvalue estimation or a polynomial transform.

Naively, preparing all $p_k(A)|v\rangle$ by independent applications costs $O(d)$ block-encoding queries and can be badly conditioned. The paper reduces this to a structured history-state problem whose cost nearly matches the appropriate Hermitian/QSVT-style degree scaling after accounting for conditioning; it is not a general removal of degree dependence.

### 2. Recurrence to linear-system reduction

For Chebyshev polynomials, the recurrence is

$$
T_{k+1}(x) = 2x T_k(x) - T_{k-1}(x).
$$

Replacing $x$ by $A$ gives

$$
T_{k+1}(A)|v\rangle = 2A T_k(A)|v\rangle - T_{k-1}(A)|v\rangle.
$$

The stacked vector of history-state components therefore satisfies a block-tridiagonal linear system over the basis register. The paper encodes this using a lower-shift operator on the degree register and applies a [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]]-style subroutine to prepare the polynomial-basis history state without treating every component as a separate transform. The resulting cost should be read with the polynomial degree, target precision, and linear-system condition number kept visible.

This is the reusable move captured by [[Generating-Function-to-QLSA Reduction for Polynomial Basis Histories]] and [[Lower-Shift Ancilla Encoding for Polynomial Degree Management]].

### 3. QEVE for real spectra

For diagonalizable matrices with real spectrum in $[-1,1]$, write $\lambda_j = \cos \theta_j$. Since

$$
T_k(\lambda_j) = \cos(k\theta_j),
$$

the Chebyshev history state is a Fourier object in the basis index $k$. A QFT on the history register extracts $\theta_j$, hence $\lambda_j$, with precision $O(1/d)$. This yields Heisenberg scaling in the target accuracy.

The price is dependence on the eigenbasis condition number when $A = V\Lambda V^{-1}$. For non-normal matrices this is unavoidable: badly conditioned eigenvectors make eigenvalue processing unstable.

### 4. QEVT for complex spectra via Faber polynomials

For spectra in a complex region $E$, Chebyshev polynomials are replaced by [[Faber-Polynomial Region Mapping for Complex-Spectrum Eigenvalue Transforms|Faber polynomials]]. If $\phi$ conformally maps the exterior of $E$ to the exterior of the unit disk, then the Faber polynomials arise from the Laurent expansion of powers of $\phi$. They play the same role on $E$ that Chebyshev polynomials play on $[-1,1]$: nearly optimal uniform approximation with bounded growth on the spectral set.

The paper shows how to prepare quantum superpositions of Faber-polynomial actions efficiently. Combined with classical approximation theory on $E$, this yields a block-encoding of $f(A)$ for polynomial $f$ with complexity nearly matching Hermitian QSVT, modulo the non-normality parameters.

### 5. Fast coefficient loading

The polynomial coefficients themselves are also prepared efficiently. Using Fourier structure of Chebyshev and related expansions, the paper shows how to generate coefficient superpositions in $O(\mathrm{polylog}(n))$ gates rather than linear cost. That coefficient-loading result is independent interest; it does not remove the polynomial degree/precision dependence of the full eigenvalue-transformation algorithm.

## Key results

### QEVT

For a block-encoded diagonalizable non-normal matrix $A$, the paper gives a framework to implement polynomial eigenvalue transformations

$$
A \mapsto f(A)
$$

for polynomial $f$, with query complexity to the block-encoding nearly recovering the Hermitian/QSVT scaling, up to dependence on non-normality measures such as the eigenbasis condition number and the geometry of the spectral set.

### QEVE

For diagonalizable matrices with real spectrum, the eigenvalue estimation algorithm achieves Heisenberg-limited scaling:

$$
\text{queries} = O\!\left(\frac{\kappa_V}{\varepsilon}\right)
$$

up to logarithmic factors and model-dependent constants, where $\kappa_V$ controls the conditioning of the eigenbasis.

### Applications

The paper derives:

- a linear differential equation solver with **strictly linear time** query complexity under the paper's average-case diagonalizable-operator assumptions,
- a ground-state preparation / energy-estimation upgrade from Hermitian matrices to diagonalizable real-spectrum matrices,
- a general framework for matrix functions of non-normal operators.

## Comparison with prior work

| Framework | Input class | What is transformed | Main limitation |
|---|---|---|---|
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] | Hermitian / walk-encoded | eigenphases of a unitary signal | does not directly handle non-normal matrices |
| [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] | block-encoded matrix | singular values | wrong object for non-normal problems |
| [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|An-Childs-Lin (2024)]] | dissipative / Laplace-representable transforms | eigenvalues via Laplace representation | different function class, not a full polynomial-processing framework |
| **This paper** | diagonalizable non-normal block-encoded matrix | eigenvalues directly | inherits non-normality conditioning penalties |

My read: this is not just “QSVT but for non-normal matrices”. The history-state / recurrence viewpoint is a different mechanism, and it opens problems that qubitization-style two-dimensional invariant subspaces simply do not see.

## Limits / caveats

- The dependence on non-normality is real. If the eigenbasis condition number is huge, the algorithm can be correspondingly bad.
- The clean QEVE guarantee is for **diagonalizable matrices with real spectra**. Jordan blocks are not handled by the main theorem.
- Faber-polynomial methods require classical information about a spectral region and its conformal map. That preprocessing is nontrivial.
- The linear-system subroutine introduces its own conditioning overheads, so the “polylog in degree” improvement does not mean the whole problem becomes easy.
- Keep three costs separate: polynomial degree or target precision, eigenbasis condition number/non-normality, and spectral-region geometry. Optimising one does not remove the others.
- For Hermitian inputs, standard QSVT is usually the cleaner tool. This paper matters when Hermiticity is genuinely absent.

## Reusable ideas

1. [[Generating-Function-to-QLSA Reduction for Polynomial Basis Histories]] — turn a polynomial recurrence into a structured linear system and prepare the whole history state at once.
2. [[Lower-Shift Ancilla Encoding for Polynomial Degree Management]] — encode basis recurrences using a shift operator on an ancilla register.
3. [[Chebyshev-State Phase Estimation for Real-Spectrum Non-Normal Inputs]] — estimate real eigenvalues by Fourier analysis of Chebyshev history states.
4. [[Faber-Polynomial Region Mapping for Complex-Spectrum Eigenvalue Transforms]] — lift Chebyshev-style approximation from intervals to complex spectral sets.
5. [[Fourier-Convolution Coefficient-State Preparation in Polylog Time]] — fast coefficient loading for polynomial transforms.

## References within this paper

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low, Wiebe (2019)]] — singular-value transformation baseline this paper extends beyond.
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — block-encoding / qubitization viewpoint for Hermitian problems.
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — Chebyshev and Fourier matrix-function techniques.
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — linear ODE motivation.
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|An-Childs-Lin (2024)]] — complementary eigenvalue-transform framework.
- Trefethen and Embree (2005), *Spectra and Pseudospectra* — classical background for non-normality and pseudospectral stability.

## Cross-links

### Paper notes
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]]
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]

### Trick cards
- [[Generating-Function-to-QLSA Reduction for Polynomial Basis Histories]]
- [[Lower-Shift Ancilla Encoding for Polynomial Degree Management]]
- [[Chebyshev-State Phase Estimation for Real-Spectrum Non-Normal Inputs]]
- [[Faber-Polynomial Region Mapping for Complex-Spectrum Eigenvalue Transforms]]
- [[Fourier-Convolution Coefficient-State Preparation in Polylog Time]]
