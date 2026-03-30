> **Source:** Juan Miguel Arrazola, Timjan Kalajdzievski, Christian Weedbrook, Seth Lloyd, *Quantum algorithm for nonhomogeneous linear partial differential equations*, Physical Review A **100**:032306, 2019; arXiv:1809.02622
> **Links:** [arXiv](https://arxiv.org/abs/1809.02622) · [PRA](https://doi.org/10.1103/PhysRevA.100.032306)
> **Tags:** #PDE #linear-systems #continuous-variable #quantum-algorithm #differential-equations #matrix-inversion #Poisson-equation

---

## The computational problem

**Input:** A linear differential operator $A = A(x_1, \ldots, x_N, \partial_{x_1}, \ldots, \partial_{x_N})$ that is a polynomial in the position variables and their partial derivatives, plus a quantum state $|f\rangle$ with wavefunction $\langle x | f \rangle = f(x)$ encoding the inhomogeneous source term.

**Output:** A quantum state $|\psi\rangle$ whose wavefunction is proportional to $\psi(x) = A^{-1}f(x)$, the solution to $A\psi(x) = f(x)$.

This is the continuous-variable (CV) analogue of [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]]: instead of inverting a matrix acting on a finite-dimensional vector, it inverts a differential operator acting on functions.

## What the paper does

Presents a quantum algorithm for inhomogeneous linear PDEs that works natively in the continuous-variable model of quantum computing. The algorithm inverts polynomial differential operators using a [[Fourier Decomposition Technique for Operator Inversion|Fourier decomposition of $A^{-1}$]] (following Childs-Kothari-Somma 2017), implemented via Hamiltonian simulation on continuous-variable modes. For fixed-degree differential operators, the runtime is polynomial in the spatial dimension $N$ — exponential improvement over classical methods that compute the full solution.

The paper also introduces exact gate decompositions for CV Hamiltonian simulation of three-mode polynomial interactions, which avoid Trotter-Suzuki commutator approximations entirely. These reduce gate counts by orders of magnitude for this class of Hamiltonians.

**My assessment:** This is a clean adaptation of the CKS Fourier-decomposition matrix inversion technique to the CV setting. The conceptual contribution is modest — it's mostly showing that the CKS trick works with position/momentum operators instead of finite-dimensional matrices. The exact decompositions are the most technically interesting part. The practical value is limited by the same issues that affect all QLSP-based PDE solvers: you need efficient state preparation for $|f\rangle$, and you only get a quantum state as output (so extracting classical information costs you). The paper is honest about these caveats but doesn't quantify the end-to-end cost as carefully as [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]] did for the FEM case.

---

## The algorithm / construction

### Step 1: Fourier decomposition of $A^{-1}$

The key identity (from Childs-Kothari-Somma 2017) is that for the odd function $g(x) = xe^{-x^2/2}$ satisfying $\int_0^\infty g(x)\,dx = 1$:

$$a^{-1} = \frac{i}{\sqrt{2\pi}} \int_{-\infty}^{\infty} dx\, \Theta(x) \int_{-\infty}^{\infty} dy\, ye^{-y^2/2} e^{-iaxy}$$

where $\Theta(x)$ is the Heaviside step function. Since $\hat{A}^{-1}$ and $e^{-i\hat{A}xy}$ are both diagonal in the eigenbasis $\{|a\rangle\}$ of $\hat{A}$, this lifts to operator inversion:

$$\hat{A}^{-1} = \frac{i}{\sqrt{2\pi}} \int dx\,dy\, \Theta(x)\, ye^{-y^2/2} e^{-i\hat{A}xy}$$

### Step 2: Resource state preparation

The algorithm requires two ancillary CV modes prepared in specific states:
- A **step function state** $|s_L\rangle = \frac{1}{\sqrt{L}} \int_0^L dx\,|x\rangle$ (a finite-width approximation to $\Theta(x)$)
- A **single photon state** $|1\rangle$, whose wavefunction $ye^{-y^2/2}$ provides the Fourier kernel

The authors use quantum neural networks (parameterised CV circuits optimised via TensorFlow/Strawberry Fields) to prepare these states. A 30-layer network achieves 99.36% fidelity for the step function state with width $L=7$ and Fock-space cutoff $d=41$.

### Step 3: Hamiltonian simulation

Apply the global unitary $e^{-i\hat{A}\hat{X}\hat{Y}}$ to all three systems (target + two ancillae), where $\hat{X}$, $\hat{Y}$ are position operators of the resource modes. This is equivalent to Hamiltonian simulation of $\hat{H} = \hat{A}\hat{X}\hat{Y}$ for unit time.

For $\hat{A}$ of the form $\hat{A} = \lambda\mathbf{1} + \sum_j (a_j\hat{X}_j + b_j\hat{P}_j + \alpha_j\hat{X}_j^2 + \beta_j\hat{P}_j^2)$ (which covers Poisson, heat, and wave equations), the Trotter-Suzuki decomposition produces terms like $e^{it\hat{X}_j\hat{X}_k\hat{X}_l}$ and $e^{it\hat{X}_j^2\hat{X}_k\hat{X}_l}$. The paper derives **exact decompositions** for these in terms of the universal CV gate set $\{e^{i\pi(\hat{X}^2+\hat{P}^2)/2}, e^{it_1\hat{X}}, e^{it_2\hat{X}^2}, e^{it_3\hat{X}^3}, e^{i\tau\hat{X}_1\hat{X}_2}\}$.

The exact decomposition of $e^{i6\delta\hat{X}_j^2\hat{X}_k\hat{X}_l}$ requires 873 universal gates, which compares favourably to commutator-approximation methods that need $\sim 2.8 \times 10^7$ gates for precision $10^{-3}$.

### Step 4: Measurement and postselection

Perform momentum homodyne measurements on both resource modes, postselecting on outcome $p = 0$. The postselected output state is $\hat{A}^{-1}_{\text{approx}}|f\rangle$, where $\hat{A}^{-1}_{\text{approx}}$ differs from $\hat{A}^{-1}$ due to:
1. **Finite step function width $L$**: introduces corrections $\sim e^{-a^2L^2/2}$ relevant only for small eigenvalues $a \lesssim 1/L$
2. **Finite measurement precision $\Delta$**: introduces corrections $\sim \Delta^3/a^3$ relevant for $a \lesssim \Delta$

The success probability scales as $O(\Delta^2)$, creating a tradeoff between approximation quality and postselection probability.

---

## Key results

**Complexity:** For a differential operator $\hat{A}$ that is a polynomial of constant degree in $N$ position/momentum operators, the Hamiltonian simulation and hence the full algorithm runs in $\text{poly}(N)$ time. This is exponential improvement over classical PDE solvers whose cost scales exponentially with dimension.

**Exact decomposition gate counts:**
| Gate | Exact decomposition | Commutator approx. ($\epsilon = 10^{-3}$) |
|---|---|---|
| $e^{i2\delta\hat{X}_j\hat{X}_k\hat{X}_l}$ | 12 gates | $\sim 10^6$ gates |
| $e^{i6\delta\hat{X}_j^2\hat{X}_k\hat{X}_l}$ | 873 gates (1749 with Fourier conjugation) | $\sim 2.8 \times 10^7$ gates |

**Approximation quality:** The function $F(a) \approx a^{-1}$ with corrections from finite $L$ (exponentially small for $a \gg 1/L$) and finite $\Delta$ (negligible for $a \gg \Delta$).

---

## Comparison with prior work

| Aspect | HHL / CKS (discrete) | This paper (CV) |
|---|---|---|
| Domain | Finite-dimensional $A\mathbf{x} = \mathbf{b}$ | Continuous $A\psi(x) = f(x)$ |
| Input | Quantum state $\|b\rangle$ | CV state with wavefunction $f(x)$ |
| Matrix inversion | Fourier decomposition of $A^{-1}$ | Same technique on differential operator |
| Hamiltonian simulation | Standard qubit methods | CV gate set + exact decompositions |
| Postselection | On ancilla register | On homodyne $p=0$ |

The discrete approach (e.g., [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang 2017]]) discretises the PDE first, then applies standard QLSA methods. This paper operates directly in the continuous domain but still requires finite approximations (step function width, measurement precision).

---

## Limits / caveats

1. **State preparation:** Assumes $|f\rangle$ can be efficiently prepared. For general source terms, this is non-trivial — the quantum neural network approach gives heuristic solutions but no guarantees.

2. **Output extraction:** The output is a quantum state, not a classical description of $\psi(x)$. Extracting useful classical information (e.g., $\int_S \psi(x)\,dx$) requires measurement and potentially many repetitions. As [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]] showed, this extraction cost can erase the exponential speedup claim.

3. **Postselection probability $O(\Delta^2)$:** Smaller measurement precision (better approximation) means lower success probability. This is the standard QLSP condition number problem in disguise — small eigenvalues of $\hat{A}$ are hard to invert.

4. **Condition number dependence:** Not explicitly analysed. The step function width $L$ effectively sets a cutoff on the smallest invertible eigenvalue ($a_{\min} \sim 1/L$), playing the role of condition number.

5. **Homogeneous part not solved:** The algorithm finds a particular solution to the inhomogeneous equation. For boundary value problems, the general solution also includes the homogeneous part. The paper suggests combining with classical homogeneous solvers but doesn't quantify this.

6. **CV model assumptions:** The universal gate set includes a cubic phase gate ($e^{it\hat{X}^3}$), which is non-Gaussian and experimentally challenging. The practical feasibility on near-term CV hardware is unclear.

---

## Reusable ideas

1. [[Fourier Decomposition Technique for Operator Inversion]] — the CKS Fourier integral identity $a^{-1} = \frac{i}{\sqrt{2\pi}}\int dx\,\Theta(x)\int dy\, ye^{-y^2/2}e^{-iaxy}$ that enables operator inversion via Hamiltonian simulation + ancilla measurement. Generic enough to work for any Hermitian operator with a spectral gap.

2. [[Exact CV Gate Decompositions for Polynomial Hamiltonians]] — exact algebraic decompositions of multi-mode polynomial CV unitaries (trilinear and quadratic-linear products) into a universal gate set, bypassing Trotter-Suzuki entirely. The key idea is using displacement conjugation: $e^{i2\hat{P}_j\hat{X}_k} e^{i\delta\hat{X}_j^3} e^{-i2\hat{P}_j\hat{X}_k} = e^{i\delta(\hat{X}_j + \hat{X}_k)^3}$ and expanding the polynomial.

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — the original quantum linear systems algorithm that this paper extends to the continuous-variable setting
- Childs-Kothari-Somma (2017) — [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|CKS]]: the Fourier decomposition technique for matrix inversion that provides the algorithmic skeleton
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]] — honest end-to-end analysis of QLSA applied to PDEs; shows extraction costs can erase exponential claims
- Lloyd-Braunstein (1999) — universality of CV quantum computation
- Sefi-van Loock (2011) — commutator approximations for CV gates that this paper's exact decompositions replace
- [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes|Leyton-Osborne (2008)]] — cited for nonlinear ODE quantum algorithms

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]]
- [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]]
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes]]

### Trick cards
- [[Fourier Decomposition Technique for Operator Inversion]]
- [[Exact CV Gate Decompositions for Polynomial Hamiltonians]]
