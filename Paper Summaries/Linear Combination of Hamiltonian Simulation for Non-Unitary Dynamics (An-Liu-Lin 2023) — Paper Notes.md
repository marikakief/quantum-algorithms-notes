> **Source:** Dong An, Jin-Peng Liu, and Lin Lin, *Linear combination of Hamiltonian simulation for nonunitary dynamics with optimal state preparation cost*, Phys. Rev. Lett. **131**, 150603 (2023), arXiv:2303.01029
> **Links:** [arXiv](https://arxiv.org/abs/2303.01029) · [PRL](https://doi.org/10.1103/PhysRevLett.131.150603)
> **Tags:** #quantum-algorithm #non-unitary #LCHS #differential-equations #hamiltonian-simulation #LCU #open-quantum-systems #Cauchy-kernel

---

## The computational problem

Simulate non-unitary dynamics governed by a general linear ODE:

$$\frac{\mathrm{d}}{\mathrm{d}t} u(t) = -A(t)\, u(t) + b(t), \quad u(0) = u_0, \quad t \in [0, T],$$

where $A(t) \in \mathbb{C}^{N \times N}$ with positive semidefinite Hermitian part $L(t) = (A(t) + A(t)^\dagger)/2 \succeq 0$. The goal: prepare a quantum state $\varepsilon$-close to $|u(T)\rangle = u(T)/\|u(T)\|$.

Two cost measures matter:
1. **Matrix queries** — calls to Hamiltonian simulation oracles $O_L(s, \tau) = e^{-iL(\tau)s}$ and $O_H(s, \tau) = e^{-iH(\tau)s}$.
2. **State-preparation queries** — calls to the oracle $O_{\mathrm{prep}}: |0\rangle \to |u_0\rangle$ and source oracle $O_b$.

The state-preparation cost is the bottleneck in prior QLSP-based approaches, where the condition number $\kappa$ of the dilated linear system blows up the query count. This paper's main contribution is removing that overhead.

---

## What the paper does

Introduces LCHS — **linear combination of Hamiltonian simulation** — a surprisingly simple identity that expresses the non-unitary propagator $\mathcal{T} e^{-\int_0^t A(s)\,ds}$ as a weighted integral over *unitary* Hamiltonian simulations:

$$\mathcal{T} e^{-\int_0^t A(s)\,ds} = \int_{\mathbb{R}} \frac{1}{\pi(1+k^2)} \mathcal{T} e^{-i\int_0^t (H(s) + kL(s))\,ds}\,dk.$$

This is a continuous [[Linear Combination of Unitaries (LCU)|LCU]] where the "unitaries" are time-dependent Hamiltonian simulations parametrised by a frequency $k$, and the weight function is the Cauchy-Lorentz distribution $1/(\pi(1+k^2))$.

The result achieves **optimal state-preparation cost** $O(q)$ where $q = (\|u_0\| + \|b\|_{L^1})/\|u(T)\|$, matching the lower bound from [An-Liu-Wang-Zhao 2022]. Prior QLSP-based methods required $O(q \cdot \|A\|T \cdot \log(1/\varepsilon))$ state-preparation queries.

**My assessment:** This is a genuinely new idea, not an incremental improvement on existing machinery. The mathematical core — a matrix Cauchy integral identity that avoids the spectral mapping theorem entirely — is elegant. The practical limitation is clear: the Cauchy kernel $1/(1+k^2)$ decays only quadratically, so the frequency cutoff $K = O(1/\varepsilon)$ makes the matrix-query cost scale as $1/\varepsilon$ rather than $\mathrm{polylog}(1/\varepsilon)$. This is the paper's main weakness, and it was the motivation for the follow-up [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (2023)]], which replaces the Cauchy kernel with a [[Near-Optimal Hardy-Space Kernel for LCHS|near-optimal Hardy-space kernel]] to get polylog precision.

Still, for problems where $L$ is fast-forwardable (e.g., diagonal matrices like complex absorbing potentials), the $K$-dependence can be absorbed into the interaction-picture Dyson series at only polylog cost. The paper demonstrates this for open quantum system dynamics, achieving near-optimal scaling in all parameters.

---

## The algorithm / construction

### Step 1: Cartesian decomposition

Write $A(t) = L(t) + iH(t)$, where $L(t) = (A + A^\dagger)/2$ (Hermitian, $\succeq 0$) and $H(t) = (A - A^\dagger)/(2i)$ (Hermitian). If $L$ is not positive semidefinite, shift: set $u(t) = e^{ct}v(t)$ so $v$ satisfies the ODE with $L(t) + cI \succeq 0$.

### Step 2: The LCHS identity (Theorem 1)

The identity generalises the scalar Fourier representation $e^{-|x|} = \int_{\mathbb{R}} \frac{1}{\pi(1+k^2)} e^{-ikx}\,dk$. For the scalar case with $H = 0$ and constant $L$, this follows from the spectral mapping theorem. The general proof — time-dependent, non-commuting $H$ and $L$ — uses a **matrix Cauchy integral theorem** without spectral mapping.

**Proof sketch (Lemma 4 in supplement):** Consider $\omega = ik$ as a complex variable. The integrand $\frac{1}{1+ik} e^{-i(H+kL)}$ is analytic in $\omega$ in the right half-plane (since $\mathrm{Re}(\omega) > 0$ gives exponential decay $e^{-\mathrm{Re}(\omega) L}$ when $L \succ 0$). Close the contour with a semicircle $\gamma_C$ in the right half-plane. On $\gamma_C$ with $R \to \infty$:
- For most angles $\theta$: the $e^{-R\cos\theta \cdot L}$ factor kills the integrand (since $L \succ 0$).
- Near $\theta = \pm \pi/2$: the arc length $\sim 1/\sqrt{R} \to 0$.

So the integral along the imaginary axis vanishes. This gives $\int_{\mathbb{R}} \frac{1}{1+ik} V(t; ik)\,dk = 0$, which, combined with the normalisation $\int \frac{dk}{\pi(1+k^2)} = 1$, yields the LCHS identity via the residue at $\omega = 1$ (i.e., $k = -i$).

The time-dependent case extends by showing $V(t; ik) = \mathcal{T}e^{-i\int_0^t(H(s)+kL(s))ds}$ is analytic in $\omega$ with the same exponential decay bound, and that $W(t) = \int \frac{1}{\pi(1+k^2)} V(t;ik)\,dk$ satisfies the same ODE as the left-hand side.

### Step 3: Discretisation and LCU implementation

Truncate the integral to $[-K, K]$ with $K = O(1/\varepsilon)$ (quadratic tail decay of Cauchy kernel) and discretise with the trapezoidal rule on $M+1$ points:

$$u(T) \approx \sum_{j=0}^{M} c_j U_j(T) u_0, \quad c_j = \frac{w_j}{\pi(1+k_j^2)}, \quad U_j(T) = \mathcal{T}e^{-i\int_0^T(H(s)+k_jL(s))ds}.$$

Each $U_j$ is a time-dependent [[Hamiltonian simulation]] problem, implementable via [[Product Formulas]] or the truncated Dyson series. All $U_j$ share the same Trotterisation structure — they differ only in the parameter $k_j$ multiplying $L$.

The SELECT oracle $\mathrm{SEL}_L(s, \tau) = \sum_j |j\rangle\langle j| \otimes e^{-iL(\tau)k_j s}$ is built with $O(\log M)$ queries to $O_L$ using binary decomposition of $k_j$ (Lemma 10). The PREPARE oracle creates $|0\rangle \to \frac{1}{\sqrt{\|c\|_1}} \sum_j \sqrt{c_j}|j\rangle$.

### Step 4: Inhomogeneous term via Duhamel's principle

For the source term $b(t)$, use:

$$\int_0^T \mathcal{T}e^{-\int_s^T A(s')\,ds'} b(s)\,ds \approx \sum_{j'=0}^{M_t} \sum_{j=0}^{M} \tilde{c}_{j,j'} \mathcal{T}e^{-i\int_{s_{j'}}^T(H(s')+k_jL(s'))ds'} |b(s_{j'})\rangle.$$

This requires a 2D trapezoidal rule and time-dependent SELECT oracles that encode different evolution intervals.

### Step 5: Amplitude amplification

The LCU succeeds with probability $\sim (\|u(T)\|/(\|c\|_1 \|u_0\|))^2$. Since $\|c\|_1 = O(1)$ (the Cauchy distribution integrates to 1), amplitude amplification boosts this at cost $O(\|u_0\|/\|u(T)\|) = O(q)$.

### Hybrid implementation

For computing observables $u(T)^* O u(T)$ without state preparation, use classical Monte Carlo sampling over the Cauchy distribution plus the non-unitary Hadamard test (Lemma 7) for each pair $(k, k')$. The sampling complexity is $O(\|O\|^2/\varepsilon^2)$ and each quantum circuit uses $O(\alpha_O/\varepsilon)$ queries.

### Interaction picture for fast-forwardable $L$

When $L$ is fast-forwardable (e.g., diagonal), switch to the interaction picture: $H_I(s; k) = e^{ikLs} H(s) e^{-ikLs}$. The LCHS becomes:

$$\mathcal{T}e^{-\int_0^t A(s)\,ds} \approx \sum_j c_j e^{-ik_jLt} \left(\mathcal{T}e^{-i\int_0^t H_I(s;k_j)ds}\right) e^{ik_jLt}.$$

Now $K$ only enters through the *derivative* of $H_I$, and the truncated Dyson series method has only logarithmic dependence on derivatives. This eliminates the $K$-induced overhead for complex absorbing potential problems.

---

## Key results

**Theorem 2 (General ODE):** State-preparation queries: $O\left(\frac{\|u_0\| + \|b\|_{L^1}}{\|u(T)\|}\right)$. Matrix queries (with $p$-th order product formula):

$$\tilde{O}\left(\left(\frac{\|u_0\| + \|b\|_{L^1}}{\|u(T)\|}\right)^{2+2/p} \frac{\Gamma_p^{1+1/p} T^{1+1/p}}{\varepsilon^{1+2/p}}\right),$$

where $\Gamma_p = \max_{0 \leq q \leq p, \tau} (\|H^{(q)}(\tau)\| + \|L^{(q)}(\tau)\|)^{1/(q+1)}$.

**Theorem 3 (Open quantum dynamics with complex absorbing potential):** For the Schrödinger equation with imaginary potential $i\partial_t u = (-\frac{1}{2}\Delta + V_R(t) - iV_I)u$, with $V_I$ diagonal and fast-forwardable:

$$\tilde{O}\left(\frac{\|u_0\|}{\|u(T)\|} T \max_t \|H(t)\| \cdot \mathrm{polylog}\frac{\max_t \|V_R'(t)\| \|V_I\|}{\varepsilon}\right)$$

matrix queries, and $O(\|u_0\|/\|u(T)\|)$ state-preparation queries. Near-optimal in all parameters.

**Optimal state-preparation cost:** The $O(q)$ state-preparation query count matches the $\Omega(q)$ lower bound from [An-Liu-Wang-Zhao 2022, Corollary 16]. Prior QLSP-based methods have $O(q \cdot \kappa \cdot \log(1/\varepsilon))$ state-preparation cost.

---

## Comparison with prior work

| Method | State-prep queries | Matrix queries (dominant term) | Avoids QLSP? |
|---|---|---|---|
| Berry (2014) — history state + HHL | $O(q \cdot \kappa \log(1/\varepsilon))$ | $\tilde{O}(\kappa T^2)$ | No |
| Berry-Costa (2022) — Dyson series | $O(q \cdot \|A\|T \log(1/\varepsilon))$ | $\tilde{O}(\lambda_A T)$ | No |
| Fang-Lin-Tong (2023) — time marching | $O(q)$ | $O(T^2)$ | Yes |
| **LCHS (this paper)** | $O(q)$ — **optimal** | $\tilde{O}(q^{2+2/p} \Gamma_p^{1+1/p} T^{1+1/p}/\varepsilon^{1+2/p})$ | **Yes** |
| An-Childs-Lin (2023) — improved LCHS | $O(q)$ | $\tilde{O}(\|A\|T \cdot \mathrm{polylog}(1/\varepsilon))$ | Yes |

The key trade-off: LCHS achieves optimal state-preparation cost but pays $O(1/\varepsilon)$ in precision for matrix queries (due to the Cauchy kernel's quadratic tail). The improved LCHS fixes this with a Hardy-space kernel.

---

## Limits / caveats

- **$O(1/\varepsilon)$ precision scaling** for matrix queries is the main weakness. The Cauchy kernel $1/(1+k^2)$ decays too slowly, requiring frequency cutoff $K = O(1/\varepsilon)$. This was later fixed by [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (2023)]].
- The $L \succeq 0$ condition is required (though always achievable via shifting). When $L$ has large negative eigenvalues, the shift constant $c$ inflates the norm and worsens the condition ratio $q$.
- The product-formula implementation introduces Trotter error scaling with $K$, making the polynomial in $\varepsilon$ worse than $1/\varepsilon$. Using truncated Dyson series improves this to logarithmic in $K$ but requires smooth Hamiltonians.
- For the hybrid implementation, the classical sampling overhead is $O(\|O\|^2/\varepsilon^2)$ — no quantum speedup in the sampling step.

---

## Reusable ideas

1. [[LCHS Kernel for Non-Unitary Dynamics]] — the Cauchy-kernel identity expressing non-unitary evolution as a continuous LCU of Hamiltonian simulations
2. [[Interaction Picture LCHS for Fast-Forwardable Dissipation]] — switching to the interaction picture when the dissipative part $L$ is fast-forwardable, to absorb the $K$-scaling into polylog Dyson series cost
3. [[Binary-Decomposed SELECT for Parameter-Dependent Unitaries]] — constructing the SELECT oracle $\sum_j |j\rangle\langle j| \otimes U_0^{k_j}$ with $O(\log M)$ queries via binary representation of $k_j$

---

## References within this paper

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — [[Linear Combination of Unitaries (LCU)|LCU]], the foundational technique that LCHS extends
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2015)]] — truncated Taylor series for near-optimal Hamiltonian simulation [8]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — Fourier/Chebyshev LCU for QLSP [10]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2017)]] — QSP for Hamiltonian simulation [11]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — QSVT [12], which LCHS explicitly avoids
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|Dong-Lin-Tong (2022)]] — QETU [13]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe-Berry-Høyer-Sanders (2010)]] — product formula for time-dependent Hamiltonians [22]
- **Low-Wiebe (2019)** — interaction picture Hamiltonian simulation [23]. Not in the vault as a standalone note.
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — improved Trotter error bounds used for the time-independent case [25]
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first quantum ODE algorithm [5]
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] — competing time-marching approach [32]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL (2009)]] — the original QLSP algorithm [4]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa-An-Sanders-Su-Babbush-Berry (2021)]] — optimal QLSA [31]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — linear ODEs via QLSP [26]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|BHMT (2002)]] — amplitude amplification [3]
- **Jin-Liu-Yu (2022)** — Schrödingerisation [41], a related but complementary approach to linearising PDEs.

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]] — the direct sequel, fixing the $1/\varepsilon$ precision scaling
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]] — extends LCHS to general eigenvalue transforms via Laplace representation
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — the foundational LCU technique
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — the Carleman approach to nonlinear ODEs (different method, same problem domain)
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]] — competing approach achieving optimal state-prep cost via different means
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — Dyson-series QLSP approach (what LCHS improves upon for state-prep)
- [[Quantum Linear Matrix Equations — Paper Notes]] — uses LCHS as a subroutine

### Trick cards
- [[LCHS Kernel for Non-Unitary Dynamics]] — the core identity
- [[Interaction Picture LCHS for Fast-Forwardable Dissipation]] — interaction picture variant
- [[Binary-Decomposed SELECT for Parameter-Dependent Unitaries]] — efficient SELECT construction
- [[Hubbard-Stratonovich Decoupling]] — related Gaussian integral trick for quadratic operator exponentials
- [[Generalised LCHS Identity]] — the generalised version with tuneable kernel (from the sequel)
- [[Near-Optimal Hardy-Space Kernel for LCHS]] — the improved kernel (from the sequel)
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]] — the alternative approach for nonlinear problems
