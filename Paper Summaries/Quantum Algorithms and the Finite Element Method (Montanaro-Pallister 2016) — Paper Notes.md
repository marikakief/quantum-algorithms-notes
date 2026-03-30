> **Source:** Ashley Montanaro, Sam Pallister, *Quantum algorithms and the finite element method*, Phys. Rev. A **93**(3):032324, 2016; arXiv:1512.05903
> **Links:** [arXiv](https://arxiv.org/abs/1512.05903) · [PRA](https://doi.org/10.1103/PhysRevA.93.032324)
> **Tags:** #QLSA #finite-element-method #PDE #linear-systems #quantum-speedup #lower-bounds

---

## The computational problem

**Input:** An elliptic second-order PDE over a $d$-dimensional domain $\Omega \subseteq \mathbb{R}^d$, with boundary conditions. A function $r : \Omega \to \mathbb{R}$.

**Output:** An $\epsilon$-approximation to the linear functional $\langle r, u \rangle = \int_\Omega r(x) u(x)\, dx$, where $u$ is the exact solution.

**Why this formulation:** The [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL algorithm]] produces a quantum state $|x\rangle \propto A^{-1}|b\rangle$ — not the classical vector. To extract useful classical information, you must measure. Computing a linear functional is the simplest such extraction, so it's the right benchmark for a fair quantum-vs-classical comparison.

## What the paper does

Works through the full complexity analysis of applying quantum linear systems algorithms to the finite element method (FEM), properly accounting for *all* contributions to the error: discretisation, linear system solving, and measurement extraction. Prior work (Clader-Jacobs-Sprouse 2013) had claimed an exponential quantum speedup but treated the system size $N$ and accuracy $\epsilon$ as independent parameters — they're not, because $N$ is chosen to achieve accuracy $\epsilon$.

**The key finding:** when you combine all error sources, the quantum speedup is polynomial, not exponential, in fixed spatial dimension with smooth solutions. The quantum advantage grows with the dimension $d$ of the PDE.

---

## The algorithm / construction

The paper is not proposing a new FEM discretisation. It asks what happens when you take a standard FEM pipeline, plug in a [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|quantum linear systems algorithm]], and then honestly account for the cost of getting a classical answer back out.

### FEM discretisation

The FEM discretises a PDE into a system of linear equations $M\tilde{u} = \tilde{f}$, where:
- $M$ is the stiffness matrix with entries $M_{ij} = a(\phi_i, \phi_j)$ (inner product from the weak formulation)
- $\tilde{f}_i = \int_\Omega f(x)\phi_i(x)\,dx$ (load vector)
- $\{\phi_i\}$ is a basis of piecewise polynomial functions on the mesh
- $M$ is sparse ($s = O(1)$ nonzeros per row), symmetric, positive semidefinite

**Discretisation error:** For piecewise degree-$k$ polynomials on a mesh with maximum edge length $h$, in $d$ dimensions:

$$\|u - \tilde{u}\| \leq C h^{k+1} |u|_{k+1}$$

where $|u|_{k+1}$ is the Sobolev $(k+1)$-seminorm. To achieve accuracy $\epsilon$, need $h = O((\epsilon/|u|_{k+1})^{1/(k+1)})$, giving $N = O(h^{-d})$ equations.

**Condition number:** For regular meshes, $\kappa(M) = O(N^{2/d})$.

---

### Classical baseline: conjugate gradient

Runtime: $O(Ns\sqrt{\kappa}\log(1/\epsilon_{CG}))$, which becomes:

| Preconditioning | Runtime |
|---|---|
| None | $\tilde{O}\left(\left(\frac{|u|_{k+1}}{\epsilon}\right)^{(d+1)/(k+1)}\right)$ |
| Optimal ($\kappa = O(1)$) | $\tilde{O}\left(\left(\frac{|u|_{k+1}}{\epsilon}\right)^{d/(k+1)}\right)$ |

The classical runtime scales as $\epsilon^{-O(d)}$ — exponentially in the dimension $d$.

---

### Quantum algorithm

Uses the [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] QLSA as the linear system solver, which produces $|x\rangle$ in $O(s\kappa\,\text{polylog}(s\kappa/\epsilon))$ queries.

Three steps, each contributing cost:

### 1. Input preparation
Prepare $|\tilde{f}\rangle$ using the Zalka/Grover-Rudolph state preparation scheme: express amplitudes as a telescoping product of cumulative weights, compute each weight efficiently. Works in $\text{poly}(\log N)$ time when $f$ is a polynomial and the mesh is regular. For general $f$, this can be expensive — see the oracle lower bound.

### 2. Linear system solving
The QLSA produces $|\tilde{u}\rangle$ approximating $M^{-1}|\tilde{f}\rangle / \|M^{-1}|\tilde{f}\rangle\|$.

Need $\|\tilde{u}\|$ separately: the HHL-based norm estimation requires $O(s\kappa^2/\delta\,\text{polylog})$ queries for relative accuracy $\delta$.

**Preconditioning:** SPAI (sparse approximate inverse) preconditioner can be used within the quantum algorithm, following Clader-Jacobs-Sprouse. Replaces $M$ with $PM$ where $P \approx M^{-1}$. If optimal ($\kappa(PM) = O(1)$), eliminates the polynomial condition-number dependence. The preconditioned input state $P|\tilde{f}\rangle$ is prepared using Chebyshev polynomial application from [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]].

### 3. Output extraction (the bottleneck)
Estimate $\langle r | \tilde{u} \rangle$ via the Hadamard test + [[Amplitude Amplification and Estimation|amplitude estimation]]: $O(1/\epsilon_{\text{out}})$ copies of $|\tilde{u}\rangle$.

The output accuracy requirement is:

$$\epsilon_L, \epsilon_{\text{out}} = O\!\left(\frac{\epsilon}{\sqrt{s}\,\|u\|_1}\right)$$

where $\|u\|_1$ is the Sobolev 1-norm. This $O(1/\epsilon)$ measurement cost is what prevents the exponential speedup — producing the quantum state is cheap ($\text{polylog}(1/\epsilon)$), but extracting classical information costs $\text{poly}(1/\epsilon)$.

### Quantum runtime

| Preconditioning | Runtime |
|---|---|
| None | $\tilde{O}\!\left(\frac{\|u\|\,|u|_{k+1}^{4/(k+1)}}{\epsilon^{(k+5)/(k+1)}} + \frac{\|u\|_1\,|u|_{k+1}^{2/(k+1)}}{\epsilon^{(k+3)/(k+1)}}\right)$ |
| Optimal ($\kappa = O(1)$) | $\tilde{O}\!\left(\frac{\|u\|_1}{\epsilon}\right)$ |

The $\epsilon$-dependence of the quantum algorithm does not depend on $d$. The classical algorithm's $\epsilon$-dependence is $\epsilon^{-O(d)}$. This is where the quantum advantage lives.

---

## Key results

$$
T_{\mathrm{quant}}=\widetilde{O}\!\left(\frac{\sqrt{s}\,\kappa\,\|u\|_1}{\epsilon}\right)
$$
for the output-extraction stage once the discretised system is in place, and in fixed spatial dimension with no preconditioning this becomes

$$
\widetilde{O}\!\left(\frac{\|u\|\,|u|_{k+1}^{4/(k+1)}}{\epsilon^{(k+5)/(k+1)}}+\frac{\|u\|_1\,|u|_{k+1}^{2/(k+1)}}{\epsilon^{(k+3)/(k+1)}}\right),
$$

while with ideal preconditioning it becomes

$$
\widetilde{O}\!\left(\frac{\|u\|_1}{\epsilon}\right).
$$

The matching classical conjugate-gradient runtimes are

$$
\widetilde{O}\!\left(\left(\frac{|u|_{k+1}}{\epsilon}\right)^{(d+1)/(k+1)}\right)
\quad\text{and}\quad
\widetilde{O}\!\left(\left(\frac{|u|_{k+1}}{\epsilon}\right)^{d/(k+1)}\right)
$$

without and with optimal preconditioning respectively.

Two conceptual results matter as much as the formulas:

1. The apparent exponential-in-$N$ QLSA gain shrinks to a polynomial-in-$\epsilon$ gain once one substitutes the discretisation relation $N = O((|u|_{k+1}/\epsilon)^{d/(k+1)})$.
2. The $O(1/\epsilon)$ output-measurement cost is not an artefact of their proof. Section IV gives black-box lower bounds showing that this overhead is basically unavoidable for generic classical-output tasks.

## Comparison with prior work

| Work | Main claim | What this paper changes |
|---|---|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] | QLSA can prepare $|A^{-1}b\rangle$ with polylogarithmic dependence on system dimension | This paper asks the harder application-level question: after discretisation and measurement, what is the true end-to-end complexity for PDE output functionals? |
| Clader-Jacobs-Sprouse (2013) | Suggested an exponential quantum speedup for FEM via preconditioned QLSA | This paper shows that once $N$ is tied to the target error $\epsilon$, the speedup is generally not exponential in fixed dimension |
| Standard conjugate gradient + FEM | Runtime polynomial in $N$ and $\sqrt{\kappa}$ | Re-expressed in terms of $\epsilon$, this becomes $\epsilon^{-O(d)}$, which is where high-dimensional quantum advantage can still survive |
| **This paper (2016)** | End-to-end quantum FEM complexity with discretisation, QLSA, and measurement all included | Separates the regimes: polynomial speedup in fixed $d$, potentially much larger gains when $d$ is large or higher derivatives are large |

The comparison the paper keeps forcing is the one earlier QLSA application papers tended to dodge: not “how fast can I prepare the solution state?” but “how fast can I obtain the classical quantity I actually asked for?” That is the right comparison, and it is why the result is more sober than the early HHL-era application claims.

### Where quantum still wins

| Regime | Quantum advantage |
|---|---|
| Fixed $d$, smooth solution, optimal preconditioning | $\tilde{O}(\|u\|_1/\epsilon)$ quantum vs $\tilde{O}((|u|_{k+1}/\epsilon)^{d/(k+1)})$ classical — polynomial speedup growing with $d$ |
| $d = 4$ (3+1 dimensions), $k = 1$, optimal preconditioning | $\tilde{O}(\|u\|_1/\epsilon)$ quantum vs $\tilde{O}((|u|_2/\epsilon)^2)$ classical — roughly quadratic |
| High $d$ (many-body, finance) | Potentially exponential in $d$, since quantum runtime's $\epsilon$-dependence is $d$-independent |
| Small $d$, very smooth solution | Quantum can be *worse* — the measurement overhead can exceed the discretisation advantage |

The quantum advantage depends on **two different** smoothness parameters: the classical runtime depends on $|u|_{k+1}$ (high derivatives), while the quantum runtime depends on $\|u\|_1$ (low derivatives). For solutions with large higher-order derivatives, quantum wins more.

---

## Lower bounds

### Black-box lower bound (Section IV A)
Any algorithm using the QLE subroutine $T$ times to distinguish states at distance $\epsilon$ satisfies:

$$\||η\rangle_{\psi,T} - |η\rangle_{\phi,T}\| \leq T\sqrt{2}\,\epsilon$$

So $T = \Omega(1/\epsilon)$ uses are needed to distinguish solutions at distance $\epsilon$. This matches the amplitude estimation cost — the $O(1/\epsilon)$ measurement overhead is tight for black-box QLE usage.

**Sharper version:** Distinguishing states with overlap $1 - \epsilon$ requires $\Omega(1/\sqrt{\epsilon})$ uses, which is tight (achieved by amplitude estimation).

### Classical replacement argument (Section IV B)
Informally: for fixed $d$ and smooth solutions, any quantum algorithm's QLE subroutine can be replaced by a classical solver with at most polynomial slowdown. The quantum advantage *doesn't come from solving the linear system faster* — it comes from the implicit state representation.

### Oracle lower bound (Section IV C)
If the input $f$ is given as a black box, solving even trivial FEM instances requires $\Omega(\sqrt{N})$ quantum queries (via reduction from unstructured search). This rules out general exponential speedups for oracle-specified inputs.

---

## Limits / caveats

- **Polynomial, not exponential, speedup in fixed dimension.** The earlier claim of Clader-Jacobs-Sprouse (2013) of exponential speedup was incomplete — it ignored the discretisation-accuracy tradeoff.
- **Preconditioning assumptions are optimistic.** The "optimal preconditioning" results assume $\kappa(PM) = O(1)$, which has no worst-case guarantee. Real performance depends on the specific PDE and mesh.
- **State preparation can be hard.** Preparing $|\tilde{f}\rangle$ efficiently requires structure in $f$ (polynomial, regularly spaced mesh). For arbitrary $f$, efficient preparation is unlikely.
- **Smoothness matters.** The quantum algorithm's advantage depends on the Sobolev norms of $u$. Rough solutions (large higher derivatives) favour quantum; very smooth solutions can favour classical.
- **Adaptive classical methods not considered.** hp-FEM and adaptive mesh refinement can achieve $\|u - \tilde{u}\| = O(e^{-1/h})$ convergence, which could close the gap in some regimes.
- **The $d$-dependent constant $C$ in the discretisation bound** is hidden in $\tilde{O}$. It may be large for high $d$, partially offsetting the exponential improvement in $\epsilon$-scaling.

---

## Reusable ideas

1. [[Discretisation-Accuracy Tradeoff in Quantum PDE Solvers]] — when a quantum algorithm solves a discretised system, the system size $N$ and the accuracy $\epsilon$ are linked: $N = O(\epsilon^{-d/(k+1)})$. An apparent exponential speedup in $N$ can become merely polynomial when re-expressed in terms of $\epsilon$. This applies to *any* quantum algorithm that solves discretised PDEs — not just FEM.

2. [[Measurement Extraction Bottleneck for QLSA Applications]] — producing the quantum state $|x\rangle \propto A^{-1}|b\rangle$ costs $\text{polylog}(1/\epsilon)$, but extracting a classical answer (even a single inner product) costs $O(1/\epsilon)$ via [[Amplitude Amplification and Estimation|amplitude estimation]]. This $O(1/\epsilon)$ is a black-box lower bound. For any QLSA application, check whether this measurement cost dominates — it often does.

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — the original QLSA; this paper analyses its application to FEM
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — the improved QLSA used for the main runtime bounds
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph (2002)]] — state preparation scheme used for $|\tilde{f}\rangle$
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude estimation for output extraction
- Clader-Jacobs-Sprouse (2013) — earlier application of QLE to FEM with incomplete complexity analysis
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett-Bernstein-Brassard-Vazirani (1997)]] — black-box lower bound technique used in Section IV A
- Cao et al. (2013) — quantum algorithm for Poisson equation via FDM

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — the QLSA that this paper applies to FEM
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] — improved QLSA used for main bounds; also provides the Chebyshev technique for applying preconditioners
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]] — state preparation for the load vector
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — another quantum PDE approach (finite difference, not FEM); similar measurement bottleneck
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — linear ODE quantum algorithm; related limitations on classical output extraction
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]] — financial applications where high-dimensional PDEs arise (Black-Scholes); related quantum advantage regime
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes]] — extends this paper's analysis to a specific PDE (heat equation), comparing 10 algorithms; confirms QLSA approaches are not competitive; at most quadratic speedup via amplitude estimation on random walks
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]] — CV version of QLSA for inhomogeneous linear PDEs; subject to the same extraction-cost limitations identified here

### Trick cards
- [[Discretisation-Accuracy Tradeoff in Quantum PDE Solvers]]
- [[Measurement Extraction Bottleneck for QLSA Applications]]
- [[Preconditioning Quantum Linear Systems via Fast Inversion]]
