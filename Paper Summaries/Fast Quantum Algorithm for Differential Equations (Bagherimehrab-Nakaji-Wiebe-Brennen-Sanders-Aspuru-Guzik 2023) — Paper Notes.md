> **Source:** Mohsen Bagherimehrab, Kouhei Nakaji, Nathan Wiebe, Gavin K. Brennen, Barry C. Sanders, Alán Aspuru-Guzik, *Fast quantum algorithm for differential equations*, arXiv:2306.11802v3 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2306.11802) · [PDF](https://arxiv.org/pdf/2306.11802)
> **Tags:** #PDE #QLSA #preconditioning #wavelets #differential-equations #linear-systems #quantum-algorithm

---

## The computational problem

Given a $d$-dimensional inhomogeneous linear PDE

$$
\mathcal{L}u(x)=b(x), \qquad x\in [0,1]^d,
$$

where $\mathcal{L}$ is a linear differential operator whose induced bilinear form

$$
B(u,v)=\int u(x)\mathcal{L}v(x)\,dx
$$

is symmetric, bounded, and coercive:

$$
B(u,v)=B(v,u),\qquad B(u,v)\le C\lVert u\rVert\lVert v\rVert,\qquad B(u,u)\ge c\lVert u\rVert^2
$$

for $0<c<C$, discretise the PDE on a $d$-dimensional grid with $N=2^{nd}$ points to obtain

$$
Au=b.
$$

**Input:** a $(1,a,0)$-[[Block-Encoding Composition Algebra|block-encoding]] $U_A$ of $A$, a state-preparation procedure $P_b$ for

$$
|b\rangle=\frac{1}{\lVert b\rVert}\sum_i b_i|i\rangle,
$$

and target precision $\varepsilon$.

**Output:** not the full classical vector $u$, but a quantum solution state $|\psi\rangle$ from which expectation values $\langle u|M|u\rangle$ of observables $M$ can be estimated.

This is a [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]]-style PDE solver, but restricted to PDEs whose discretised operators admit wavelet preconditioning with condition number independent of $N$.

## What the paper does

The paper gives a quantum PDE solver whose cost is polylogarithmic in the grid size $N$ and independent of the original finite-difference condition number $\kappa(A)$ for a broad class of elliptic PDEs. The trick is to move the linear system into a wavelet basis, apply a diagonal wavelet preconditioner so the new matrix has $\kappa=O(1)$, and avoid the usual postselection penalty from the non-unitary preconditioner by estimating observables in a two-ancilla extended space.

**My assessment:** this is one of the more useful Sanders-adjacent papers for this vault. It sits exactly at the intersection of [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]], PDE algorithms, preconditioning, and simulation-style basis changes. The claim is not a generic escape from condition-number lower bounds; it works because the PDE class has classical wavelet structure that the quantum algorithm can exploit.

---

## The algorithm / construction

### 1. Start from a standard finite-difference discretisation

The algorithm does **not** require discretising the PDE in a wavelet basis. It can start with a conventional finite-difference system

$$
Au=b,
$$

where $A\in\mathbb{R}^{N\times N}$, $u,b\in\mathbb{R}^N$, and $N=2^{nd}$. For second-order differential operators, the condition number of $A$ often grows polynomially with $N$; the paper notes the typical $\kappa(A)\sim N^2$ behaviour.

A direct application of [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|high-precision QLSA]] or [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] would still pay at least linearly in this condition number for worst-case linear systems.

### 2. Change to a wavelet basis

Apply the quantum wavelet transform $W$ to get

$$
u_W=Wu,\qquad b_W=Wb,\qquad A_W=WAW^T.
$$

The wavelet transform is used as an auxiliary computational basis, not necessarily as the original discretisation. Existing quantum wavelet transforms implement $W$ on $n$ qubits with $O(n^2)$ gates; in $d$ dimensions, $W_{dD}=\bigotimes_{i=1}^d W$ gives cost $O(dn^2)$.

### 3. Apply the wavelet preconditioner

In the wavelet basis, use a diagonal preconditioner $P$:

$$
A_P=P A_W P,\qquad b_P=P b_W,\qquad u_P=P^{-1}u_W.
$$

Classical wavelet theory says that for the symmetric/bounded/coercive PDE class, this preconditioner is optimal: $\kappa(A_P)$ is bounded independently of $N$ and depends only on the wavelet family.

For the one-dimensional case, the preconditioner acts as

$$
P|j\rangle=2^{-\lfloor \log_2 j\rfloor}|j\rangle,
$$

with no action on $|0\rangle$. For $d$ dimensions, Lemma 1 gives

$$
P_{dD}|j_1\rangle\cdots |j_d\rangle
=2^{-\lfloor \log_2 j_{\max}\rfloor}|j_1\rangle\cdots |j_d\rangle,
$$

where $j_{\max}=\max(j_1,
\ldots,j_d)$, with trivial action when $j_{\max}=0$.

### 4. Avoid directly applying the non-unitary preconditioner

The naive route would be

$$
|u\rangle=W^\dagger P A_P^{-1} P W |b\rangle.
$$

This fails as a fast algorithm because applying $P$ probabilistically has success probability controlled by $\lambda_{\min}(P)^2=(2/N)^2$, so amplitude amplification costs $\Omega(N)$.

The paper avoids this by decomposing

$$
P=\frac{U^++U^-}{2},\qquad
U^\pm=P\pm i\sqrt{I-P^2}=e^{\pm i\arccos P}.
$$

These $U^\pm$ are diagonal unitaries with the same block-scale structure as $P$. In one dimension,

$$
U^\pm=\prod_{r=1}^{n-1}\Lambda^{r-1}_0(R_z(\pm\theta_{n-r}))\otimes I_{n-r},
\qquad \theta_r=\arccos(2^{-r}).
$$

Lemma 2 implements controlled-$U^\pm$ using $O(n)$ Toffoli, one-qubit, and two-qubit gates plus $n$ ancillas. In $d$ dimensions, the circuit first computes $j_{\max}$ using $O(dn)$ Toffoli gates and $O(n)$ ancillas, applies the corresponding phase, then uncomputes.

### 5. Prepare an extended solution state

Define, for $a,b\in\{0,1\}$ and $U^{0/1}\equiv U^{+/-}$,

$$
|\psi_{ab}\rangle=W^\dagger U^a A_P^{-1}U^b W|b\rangle.
$$

Then

$$
|u\rangle=\frac{1}{4}\sum_{a,b\in\{0,1\}}|\psi_{ab}\rangle.
$$

Instead of preparing $|u\rangle$ directly, the circuit prepares

$$
|\psi\rangle=\frac{1}{2\xi}\sum_{a,b\in\{0,1\}}|ab\rangle|\psi_{ab}\rangle,
\qquad
\xi^2=\frac{1}{4}\sum_{a,b}\lVert |\psi_{ab}\rangle\rVert^2.
$$

For an observable $M$ on the original solution space, define the extended observable

$$
M'=\sum_{a,b,c,d\in\{0,1\}} |ab\rangle\langle cd|\otimes M.
$$

Then

$$
\langle \psi|M'|\psi\rangle=\frac{4}{\xi^2}\langle u|M|u\rangle.
$$

The paper shows that sparse-access oracles for $M'$ can be built with one query to the oracles for $M$, because $M'$ is a $4\times4$ block matrix with $M$ in every block.

### 6. Block-encode the inverse of the preconditioned matrix

The circuit uses a [[Block-Encoding Composition Algebra|block-encoding]] of

$$
A_P^{-1}=(P W A W^\dagger P)^{-1}.
$$

A block-encoding of $A_P$ is built from one use of $U_A$ plus $O(n^2)$ gates for the wavelet transform and preconditioner unitaries. Since $\kappa(A_P)=O(1)$, existing QLSAs construct the inverse block-encoding with

$$
O(\log(1/\varepsilon))
$$

uses of $U_A$.

Amplitude amplification succeeds with $O(1)$ rounds on average because the success probability of the inverse block-encoding is constant once $A_P$ has constant condition number.

### 7. Boundary conditions and direct wavelet discretisation

The main theorem assumes periodic boundaries so that the standard wavelet transform applies cleanly. For non-periodic boundaries, the paper suggests either boundary wavelets / incomplete wavelet transforms or a method-of-images periodic extension.

The authors also describe a fully wavelet-discretised version. If $A$ and $b$ are given directly in the wavelet basis, the $O(n^2)$ QWT cost disappears and the gate cost drops to $O(n)$, but this shifts the burden to constructing $U_A$ and $P_b$ in that basis.

## Key results

**Theorem 1.** Let $\mathcal{L}u(x)=b(x)$ be a $d$-dimensional inhomogeneous linear PDE on $[0,1]^d$ with periodic boundaries, and assume the bilinear form induced by $\mathcal{L}$ is symmetric, bounded, and elliptic/coercive. Let $Au=b$ be the finite-difference linear system on a $d$-dimensional grid with $N=2^{nd}$ points. Given a $(1,a,0)$-block-encoding $U_A$ of $A$ and a procedure $P_b$ generating $|b\rangle$, an $\varepsilon$-approximation of the solution state $|\psi\rangle$ can be generated using

$$
O(1)
$$

uses of $P_b$,

$$
O(\log(1/\varepsilon))
$$

uses of $U_A$, and

$$
O(dn^2)=O\!\left(d\log^2(N^{1/d})\right)
$$

gates, all on average.

**Lemma 1.** The $d$-dimensional wavelet preconditioner acts by the largest scale index:

$$
P_{dD}|j_1\rangle\cdots |j_d\rangle
=2^{-\lfloor\log_2 j_{\max}\rfloor}|j_1\rangle\cdots |j_d\rangle.
$$

**Lemma 2.** Controlled-$U^\pm$ for the one-dimensional preconditioner can be implemented with $O(n)$ Toffoli, one-qubit, and two-qubit gates, using $n$ ancillas.

**Lemma 3.** The inverse block-encoding of $A_P^{-1}$ can be executed using $O(\log(1/\varepsilon))$ uses of the block-encoding of $A$ and $O(n^2)$ gates.

**Lemma 4.** Amplitude amplification costs $O(1)$ calls to $P_b$, $O(\log(1/\varepsilon))$ uses of $U_A$, and $O(n^2)$ gates on average.

**Lemma 5.** The $j_{\max}$ operation needed for the multidimensional preconditioner costs $O(dn)$ Toffoli gates and $O(n)$ ancillas.

## Comparison with prior work

| Approach | Main idea | Condition-number issue | Scaling highlighted in this note |
|---|---|---|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL / generic QLSA]] | invert $A$ from block/sparse access | pays at least linear $\kappa$ for general systems | not fast when PDE discretisation has $\kappa=\mathrm{poly}(N)$ |
| [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes|Childs-Liu-Ostrander 2021]] | spectral discretisation for PDEs | condition number controlled by spectral structure, but still analysed through QLSA condition bounds | polylog precision for smooth elliptic PDEs |
| [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes|Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019]] | CV operator inversion via Fourier decomposition | cutoff / postselection acts like a hidden small-eigenvalue bottleneck | continuous-variable model; condition-number dependence not removed |
| [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes|Tong-An-Wiebe-Lin 2021]] | precondition using a fast-invertible dominant part | advantage depends on structured splitting $A+B$ | reduces dependence on the hard part of the matrix |
| This paper | wavelet-basis diagonal preconditioning for elliptic PDEs | $\kappa(A_P)=O(1)$ for the PDE class | $O(\log(1/\varepsilon))$ queries to $U_A$, $O(dn^2)$ gates |

## Limits / caveats

1. **Not a generic QLSA speedup.** The condition-number lower bound for arbitrary linear systems is untouched. The escape hatch is the PDE promise: wavelets give an $N$-independent preconditioner for this operator class.

2. **Restricted PDE class.** The bilinear form must be symmetric, bounded, and coercive. The paper's examples include Sturm-Liouville problems, Poisson-type equations, Helmholtz-type equations under the right assumptions, time-independent Schrödinger-type equations, and biharmonic equations. Hyperbolic, strongly non-normal, nonlinear, or shock-forming PDEs are outside the scope.

3. **Quantum-state output.** As with most [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] applications, the algorithm produces a state for estimating features. Reading out the full solution vector costs $\Omega(N)$.

4. **Observable estimation remains separate.** The theorem prepares $|\psi\rangle$; estimating $\langle u|M|u\rangle$ then needs the usual sampling / amplitude-estimation overhead, plus control of the normalisation factor $\xi^2$.

5. **Input model matters.** The result assumes efficient $P_b$ and a block-encoding of the finite-difference matrix $A$. For arbitrary coefficient functions or boundary data, constructing these oracles can be the real cost.

6. **Boundary treatment is sketched rather than central.** Periodic boundaries are clean. Non-periodic boundaries require boundary wavelets, incomplete transforms, or method-of-images extensions; those choices can affect constants and implementation detail.

## Reusable ideas

1. [[Wavelet Preconditioning for Quantum PDE Solvers]] — use a wavelet basis only as an auxiliary preconditioning basis, so a standard finite-difference discretisation can still feed a QLSA with an $N$-independent effective condition number.

2. [[Extended Observable Trick for Nonunitary Preconditioners]] — avoid postselecting a badly conditioned non-unitary map by decomposing it into a pair of unitaries and estimating the desired observable in an ancilla-extended space.

3. [[Max-Scale Diagonal Preconditioner for Multidimensional Wavelets]] — implement the $d$-dimensional wavelet preconditioner by computing the largest scale index $j_{\max}$, applying a scale-dependent phase, and uncomputing.

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — original QLSA and the condition-number bottleneck this paper targets for PDE-structured instances.
- Ambainis (2012) — variable-time amplitude amplification for linear algebra.
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2017)]] — high-precision QLSA via Fourier/Chebyshev LCU.
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — QSVT machinery for inverse block-encoding.
- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|Lin-Tong (2020)]] — eigenstate filtering / QLSA approach.
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa-An-Sanders-Su-Babbush-Berry (2022)]] — optimal-scaling discrete-adiabatic QLSP solver.
- [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes|Childs-Liu-Ostrander (2021)]] — high-precision PDE algorithms; important comparison point.
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — early quantum linear differential-equation solver.
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]] — finite-element PDE solver and cautionary end-to-end readout analysis.
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes|Arrazola-Kalajdzievski-Weedbrook-Lloyd (2019)]] — CV PDE solver via operator inversion.
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes|Childs-Liu (2020)]] — spectral differential-equation methods.
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes|Krovi (2023)]] — later ODE/nonlinear-DE solver improvements.
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2007)]] — cited for the no-fast-forwarding comparison.
- Brennen-Rohde-Sanders-Singh (2015) — multiscale QFT simulation using wavelets; close precursor for the wavelet/QFT side, not yet in this vault.
- Bagherimehrab-Y. Sanders-Berry-Brennen-B. Sanders (2022) — free-QFT ground-state generation using wavelets; also a good future vault candidate.
- Alase-Nerem-Bagherimehrab-Høyer-Sanders (2022) — expectation-value estimation from a system of linear equations.
- Bagherimehrab-Aspuru-Guzik (2024) — efficient quantum algorithms for quantum wavelet transforms.
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes|Tong-An-Wiebe-Lin (2021)]] — quantum preconditioning via fast inversion.

## Cross-links

### Paper notes

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]]
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]]
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]]
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]]
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]]
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]

### Trick cards

- [[Wavelet Preconditioning for Quantum PDE Solvers]]
- [[Extended Observable Trick for Nonunitary Preconditioners]]
- [[Max-Scale Diagonal Preconditioner for Multidimensional Wavelets]]
- [[Block-Encoding Composition Algebra]]
- [[Preconditioning Quantum Linear Systems via Fast Inversion]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Measurement Extraction Bottleneck for QLSA Applications]]
