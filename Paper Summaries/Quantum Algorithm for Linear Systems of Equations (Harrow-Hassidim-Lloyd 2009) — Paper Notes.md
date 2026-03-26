> **Source:** Aram W. Harrow, Avinatan Hassidim, and Seth Lloyd, *Quantum algorithm for linear systems of equations*, arXiv:0811.3171, Phys. Rev. Lett. **103**, 150502 (2009)
> **Links:** [arXiv](https://arxiv.org/abs/0811.3171) · [PRL](https://doi.org/10.1103/PhysRevLett.103.150502)
> **Tags:** #linear-systems #QLSA #phase-estimation #hamiltonian-simulation #BQP-complete

---

## What the paper does

Gives the first quantum algorithm for solving linear systems of equations with exponential speedup in the system dimension $N$. Given an $s$-sparse $N \times N$ matrix $A$ with condition number $\kappa$, produces a quantum state $|x\rangle \propto A^{-1}|b\rangle$ in time $\tilde{O}(\log(N) s^2 \kappa^2 / \epsilon)$, versus $O(Ns\sqrt{\kappa}\log(1/\epsilon))$ classically.

The speedup is exponential in $N$ but polynomial in $\kappa$ — so the advantage is greatest when $N$ is huge and $\kappa$ is small.

The paper also proves matrix inversion is **BQP-complete**: improving the $\kappa$-dependence to $\kappa^{1-\delta}$ for any $\delta > 0$ would imply BQP = PSPACE.

---

## The computational problem

**Input:** An $s$-sparse, row-computable Hermitian $N \times N$ matrix $A$ with $\kappa^{-1} I \leq A \leq I$, and a procedure to prepare $|b\rangle$.

**Output:** A quantum state $|x\rangle$ such that $\||x\rangle - A^{-1}|b\rangle / \|A^{-1}|b\rangle\|\| \leq \epsilon$.

**Complexity measure:** Number of gates / queries to $A$ and the state preparation procedure for $|b\rangle$.

The paper emphasizes that $|x\rangle$ is useful not for reading out all of $\vec{x}$, but for estimating expectation values $\langle x|M|x\rangle$ — things like normalization, weights, moments.

---

## The algorithm

### Step 1: Hamiltonian simulation

Use $A$ as a Hamiltonian and simulate $e^{iAt}$ via [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|sparse Hamiltonian simulation]]. If $A$ is $s$-sparse, this costs $\tilde{O}(\log(N) s^2 t)$ per application.

If $A$ is not Hermitian, embed it as:

$$
C = \begin{pmatrix} 0 & A \\ A^\dagger & 0 \end{pmatrix}
$$

and solve $C\vec{y} = \binom{\vec{b}}{0}$. This doubles the dimension but makes the problem Hermitian.

### Step 2: Phase estimation

Prepare the clock state:

$$
|\Psi_0\rangle = \sqrt{\frac{2}{T}} \sum_{\tau=0}^{T-1} \sin\frac{\pi(\tau + \frac{1}{2})}{T} |\tau\rangle
$$

The sinusoidal weighting (from Luis-Peřina / Buzek-Derka-Massar) minimizes a quadratic loss function that controls the phase estimation error.

Apply conditional Hamiltonian evolution $\sum_\tau |\tau\rangle\langle\tau| \otimes e^{iA\tau t_0/T}$ on $|\Psi_0\rangle \otimes |b\rangle$, then Fourier transform the clock register. This decomposes $|b\rangle = \sum_j \beta_j |u_j\rangle$ in the eigenbasis of $A$ and writes the eigenvalues $\tilde{\lambda}_k \approx \lambda_j$ into the clock register:

$$
\sum_{j=1}^{N} \sum_{k=0}^{T-1} \alpha_{k|j} \beta_j |\tilde{\lambda}_k\rangle |u_j\rangle
$$

### Step 3: Conditional rotation (the inversion step)

Adjoin an ancilla qubit and rotate conditioned on $\tilde{\lambda}_k$:

$$
|\tilde{\lambda}_k\rangle |u_j\rangle \mapsto |\tilde{\lambda}_k\rangle |u_j\rangle \left(\sqrt{1 - \frac{C^2}{\tilde{\lambda}_k^2}}|0\rangle + \frac{C}{\tilde{\lambda}_k}|1\rangle\right)
$$

where $C = O(1/\kappa)$.

### Step 4: Uncompute and postselect

Uncompute the eigenvalue register (reverse phase estimation). Measure the ancilla — conditioned on outcome $|1\rangle$, the state is proportional to:

$$
\sum_{j=1}^{N} \beta_j \frac{C}{\lambda_j} |u_j\rangle \propto A^{-1}|b\rangle
$$

Success probability: $\Omega(1/\kappa^2)$. Use [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]] to boost this with $O(\kappa)$ repetitions.

```
|b⟩ ─────┤ Phase Est. ├──┤ Rotate C/λ ├──┤ Uncompute ├──┤ Measure ├── |x⟩
          │            │  │  ancilla   │  │  eigenvals │  │ ancilla │
|Ψ₀⟩ ────┤  e^{iAt}   ├──┤            ├──┤   (reverse)├──┘         │
          │            │  │ conditioned│  │            │            │
          └────────────┘  │  on λ̃_k   │  └────────────┘            │
                          └────────────┘                            │
                                                    Postselect |1⟩ ─┘
                                                    Prob ≥ 1/κ²
                                                    Boost with amp. amp.
```

### Step 5: Extract information

Don't read out all of $|x\rangle$ (that takes $O(N)$ time). Instead measure an observable $M$ to estimate $\langle x|M|x\rangle$.

---

## Complexity

$$
\tilde{O}\!\left(\log(N) \cdot s^2 \cdot \kappa^2 / \epsilon\right)
$$

Breaking this down:
- $\log(N)$: from Hamiltonian simulation (exponential speedup over classical $O(N)$)
- $s^2$: from [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|sparse simulation]] sparsity dependence
- $\kappa^2$: $\kappa$ from amplitude amplification, another $\kappa$ from $t_0 = O(\kappa/\epsilon)$
- $1/\epsilon$: from phase estimation precision needed for eigenvalue inversion

The $\kappa^2$ dependence was later improved to $\kappa$ by [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2017)]] using [[Variable-Time Amplification for Condition-Number Reduction|variable-time amplitude amplification]], and the $1/\epsilon$ to $\mathrm{poly}\log(1/\epsilon)$ using [[Linear Combination of Unitaries (LCU)|LCU]] / [[QSVT Meta-Template|QSVT]] approaches.

---

## BQP-completeness (Theorem 4)

Matrix inversion is BQP-complete. The reduction embeds an arbitrary $n$-qubit, $T$-gate quantum circuit into a matrix inversion instance:

Define unitary $U = \sum_{t=1}^{T} |t+1\rangle\langle t| \otimes U_t + \cdots$ (cyclic, period $3T$), then set $A = I - Ue^{-1/T}$. This gives:

$$
A^{-1} = \sum_{k \geq 0} U^k e^{-k/T}
$$

which applies $U^t$ for a geometrically distributed $t$. Measuring the time register and obtaining $T+1 \leq t \leq 2T$ (probability $\geq 1/10$) yields the circuit output.

**Consequences:**
- $A$ has $\kappa = O(T)$, dimension $N = O(2^n T)$
- A classical $\mathrm{poly}(\log N, \kappa)$ solver would simulate BQP in polynomial time → BPP = BQP (unlikely)
- A quantum algorithm with $\kappa^{1-\delta}$ dependence would give BQP = PSPACE
- Improving $\epsilon$ dependence to $\mathrm{poly}\log(1/\epsilon)$ would give BQP ⊇ PP

This means the $\kappa^2$ dependence is essentially tight (up to the linear improvement to $\kappa$), and the $1/\epsilon$ polynomial dependence is unavoidable for sampling-based algorithms.

---

## Handling ill-conditioned matrices

The algorithm uses **filter functions** $f(\lambda)$, $g(\lambda)$ to split the spectrum:

$$
|u_j\rangle \mapsto |u_j\rangle \left(\sqrt{1 - f(\lambda_j)^2 - g(\lambda_j)^2}\,|\text{nothing}\rangle + f(\lambda_j)\,|\text{well}\rangle + g(\lambda_j)\,|\text{ill}\rangle\right)
$$

- $f(\lambda) = 1/(C\kappa\lambda)$ for $\lambda \geq 1/\kappa$ (inversion)
- $g(\lambda) = 1/C$ for $\lambda \leq 1/\kappa'$ (flag as ill-conditioned)
- Smooth interpolation in between

The user can choose to work only with the well-conditioned part, or estimate the weight in the ill-conditioned part.

---

## Where this sits historically

| Paper | $\kappa$ dependence | $\epsilon$ dependence | Key technique |
|---|---|---|---|
| **This paper (HHL 2009)** | $\kappa^2$ | $1/\epsilon$ | [[Gapped Phase Estimation\|Phase estimation]] + conditional rotation |
| [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes\|Childs-Kothari-Somma (2017)]] | $\kappa$ | $\mathrm{poly}\log(1/\epsilon)$ | [[Fourier Integral LCU for Matrix Inversion\|Fourier/Chebyshev LCU]] + [[Variable-Time Amplification for Condition-Number Reduction\|variable-time amplification]] |
| [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes\|Gilyén et al. (2019)]] | $\kappa$ | $\mathrm{poly}\log(1/\epsilon)$ | [[QSVT Meta-Template\|QSVT]] polynomial inversion |

---

## Fine print (Aaronson's caveats)

Scott Aaronson's essay [*Quantum Machine Learning Algorithms: Read the Fine Print*](https://www.scottaaronson.com/papers/qml.pdf) (Nature Physics **11**, 291, 2015) lays out the four ways the "exponential speedup" can silently vanish:

1. **State preparation.** Loading $|b\rangle$ from classical data requires either a QRAM or a structured formula for the amplitudes. If preparing $|b\rangle$ takes $n^c$ time, the exponential advantage is gone at step one.

2. **Hamiltonian simulation access.** Applying $e^{iAt}$ requires $A$ to be sparse (or have other special structure) with efficiently-queryable entries. For dense or unstructured $A$, this step alone kills the speedup.

3. **Condition number.** HHL's runtime grows linearly in $\kappa$. If $\kappa = n^c$, the advantage disappears. The BQP-completeness result (Theorem 4 above) shows that improving $\kappa$-dependence below linear would have complexity-theoretic consequences.

4. **Readout.** The output is a quantum state $|x\rangle$, not the vector $x$. You can extract limited statistical information (inner products, locations of large entries) but reading any specific $x_i$ requires $\sim n$ repetitions — exponential overhead that negates the exponential speedup.

Aaronson's broader point: every QML algorithm built on HHL inherits these caveats. The "task" that HHL solves efficiently is not "solve $Ax = b$" but rather "estimate certain observables of $A^{-1}|b\rangle$ for structured $A$ and efficiently-preparable $|b\rangle$." Whether classical algorithms can match that narrower task remains open for most applications. The one provable separation comes from HHL's BQP-completeness: encoding Shor's algorithm into a linear system gives a case where classical solvers provably can't compete (assuming factoring is hard classically), but those instances are artificial.

The essay also highlights forrelation (Aaronson-Ambainis 2014) as a cleaner separation: comparing a vector against the Fourier transform of another vector gives provable exponential quantum speedup, but only when both vectors are sufficiently uniform and the relationship involves a global transform that foils classical sampling.

---

## Reusable ideas

1. **Phase-estimation-based eigenvalue extraction:** Decompose $|b\rangle$ in the eigenbasis of $A$ and write eigenvalues into a register. Then apply any function $f(\lambda)$ conditioned on the eigenvalue register. This is the template for all phase-estimation-based quantum linear algebra — later superseded by [[QSVT Meta-Template|QSVT]] but still the clearest conceptual framework.

2. **Conditional rotation for matrix inversion:** Rotate an ancilla by angle $\arcsin(C/\lambda)$ to encode $\lambda^{-1}$ in the amplitude. Postselection on the ancilla gives $A^{-1}|b\rangle$. Simple but lossy (success probability $1/\kappa^2$).

3. **BQP-completeness via circuit-to-matrix reduction:** Embed a quantum circuit into a sparse matrix $A = I - Ue^{-1/T}$ whose inverse $A^{-1} = \sum_k U^k e^{-k/T}$ applies a geometrically weighted sum of circuit steps. This is both the lower bound proof and a conceptual connection: matrix inversion is as hard as quantum computation itself.

4. **Filter functions for spectral splitting:** Use smooth $f(\lambda), g(\lambda)$ to split eigenspaces into well-conditioned and ill-conditioned parts, flagging each with an ancilla. The Lipschitz continuity of $|h(\lambda)\rangle$ controls the phase-estimation error propagation.

---

## References within this paper

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2007)]] — sparse Hamiltonian simulation used for $e^{iAt}$
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov & Ta-Shma (2003)]] — additional Hamiltonian simulation techniques
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — eigenvalue measurement (phase estimation foundation)
- Luis & Peřina (1996), Buzek, Derka & Massar (1999) — optimized phase estimation with sinusoidal weights
- Cleve, Ekert, Macchiavello & Mosca (1998) — quantum phase estimation
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]] and [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]
- Grover & Rudolph (2002) — efficient state preparation for integrable distributions
- Shewchuk (1994) — conjugate gradient method (classical comparison)
- Simon (1997) — Simon's problem (used in oracle lower bounds)
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — first exponential quantum speedup; established the Fourier-sampling paradigm
- Aaronson, [*Quantum Machine Learning Algorithms: Read the Fine Print*](https://www.scottaaronson.com/papers/qml.pdf), Nature Physics **11**, 291 (2015) — the definitive caveat-emptor on HHL-based QML claims
- Clader, Jacobs & Sprouse, [*Preconditioned quantum linear system algorithm*](https://arxiv.org/abs/1301.2340), PRL **110**, 250504 (2013) — addresses state preparation (oracle-controlled rotation), readout (ancilla measurement for observables), and condition number (SPAI preconditioner); EM scattering cross-section as application

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]] — extends HHL to rectangular (non-square) least-squares via isometry embedding; three algorithms for parameter state prep, fit quality estimation, and sparse classical readout
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] — improves $\kappa^2 \to \kappa$ and $1/\epsilon \to \mathrm{poly}\log(1/\epsilon)$
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — [[QSVT Meta-Template|QSVT]] subsumes the HHL approach
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — uses QLSA as subroutine for ODEs
- [[Quantum Linear Matrix Equations — Paper Notes]] — matrix equation generalization
- Clader-Jacobs-Sprouse (2013), arXiv:1301.2340 — preconditioned QLSA; oracle-based state prep, SPAI preconditioning, EM scattering application
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — preconditioned QLSP via block-encoding + QSVT; supersedes Clader et al.
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — Hamiltonian simulation backbone
- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes|Lloyd, Mohseni & Rebentrost (2014)]] — extends HHL ideas; [[Density Matrix Exponentiation via Partial Swap|density matrix exponentiation]] for non-sparse matrices
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] — improves to near-optimal $O(\kappa\,\mathrm{polylog}(\kappa/\varepsilon))$ via adiabatic scheduling; direct successor to HHL
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — achieves the optimal $O(\kappa\log(1/\varepsilon))$ lower bound via discrete adiabatic theorem; the definitive successor to HHL
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — numerical validation showing the optimal QLSP solver's practical constant is ~1,500× smaller than the analytical bound
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — argues QLSP-based ODE simulation is suboptimal for oscillator dynamics; exponential speedup requires bypassing HHL-style linear system formulations
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — early ODE solver that directly uses HHL as its linear-system subroutine
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — later ODE solver built on the same HHL/QLSP pipeline, extended to time-dependent coefficients via Dyson series
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — downstream adiabatic-simulation result in the same Costa-An-Berry lineage that grew out of improving HHL-style QLSP dependence

### Trick cards
- [[Gapped Phase Estimation]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Variable-Time Amplification for Condition-Number Reduction]]
- [[Fourier Integral LCU for Matrix Inversion]]
- [[QSVT Meta-Template]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — nonlinear ODEs via Carleman linearisation + QLSP; demonstrates the QLSP-based pipeline for a qualitatively different (nonlinear) problem class
