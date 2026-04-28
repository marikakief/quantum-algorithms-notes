> **Source:** Abtin Ameri, Euan Carolan, Andrew M. Childs, Nolan Krovi, *Quantum lower bounds for simulating fluid dynamics*, arXiv:2603.12161 (2026)
> **Links:** [arXiv](https://arxiv.org/abs/2603.12161)
> **Tags:** #lower-bound #fluid-dynamics #nonlinear-PDEs #polynomial-method #KdV #Euler-equations #quantum-advantage #copies-lower-bound

---

## The computational problem

**Setting:** Can quantum computers significantly outperform classical algorithms for simulating fluid dynamics? Many quantum algorithms for fluid simulation have been proposed — lattice Boltzmann linearisation, Carleman embedding, direct PDE solvers — but it has remained unclear whether genuine speedups are achievable in practice or in principle.

This paper asks: for specific, physically important fluid models, what is the minimum resource cost (measured in **copies of the initial state**) that any quantum simulation algorithm must pay? The focus is on two models:

1. **Korteweg–de Vries (KdV) equation:** $\partial_t u + 6u \partial_x u + \partial_{xxx} u = 0$, a nonlinear PDE modelling shallow water waves and solitons.
2. **Incompressible Euler equations:** $\partial_t \mathbf{u} + (\mathbf{u} \cdot \nabla) \mathbf{u} = -\nabla p$, $\nabla \cdot \mathbf{u} = 0$, modelling ideal inviscid fluid flow.

Both are nonlinear. The KdV equation is integrable with stable soliton solutions; the Euler equations are far more complex and can exhibit finite-time blow-up candidates in 3D.

---

## What the paper does

Proves **information-theoretic lower bounds** on the number of copies of the initial quantum state that any quantum algorithm must use to simulate these fluid equations for time $T$. The copy complexity lower bound is independent of the computational model (gate-based, analog, etc.) — it applies to *any* quantum algorithm.

The technique is a nonlinear extension of the **polynomial method** applied to quantum state copies. The key idea: a quantum algorithm that produces an $\varepsilon$-approximation to the output state from $k$ copies of the input state can be viewed as a degree-$k$ polynomial map on the input density matrix. Nonlinear dynamics with exponential sensitivity (Euler) or polynomial-growth sensitivity (KdV) then forces $k$ to be large.

The paper directly confronts the optimistic claims in the literature — in particular, the proposal of [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021]] and adjacent work — by showing that for the most physically relevant nonlinear regimes, no quantum algorithm can be efficient without an exponential number of copies (for Euler) or at least $\Omega(T^2)$ copies (for KdV).

---

## The algorithm / construction

This is a lower bounds paper; there is no new algorithm. The proof structure is:

**Step 1 — Polynomial representation of quantum algorithms with $k$ copies.**
Any quantum algorithm acting on $k$ copies of an unknown state $|\psi\rangle$ produces an output that is a degree-$k$ polynomial in the entries of the density matrix $\rho = |\psi\rangle\langle\psi|$. This is a standard observation (related to the [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|polynomial method]] in query complexity, adapted to the state-copy setting).

**Step 2 — Lower bounding the required polynomial degree.**
For the KdV equation, the solution $u(T)$ depends on the initial data $u(0)$ in a way that requires a polynomial of degree $\Omega(T^2)$ to approximate to constant accuracy in the worst case. This exploits the nonlinear growth structure of KdV.

For the Euler equations, the exponential sensitivity of solutions (Lyapunov-like instability) forces the required polynomial degree to grow as $e^{\Omega(T)}$.

**Step 3 — Combining.**
Since any $k$-copy quantum algorithm corresponds to a degree-$k$ polynomial map, Steps 1–2 together imply $k \geq \Omega(T^2)$ for KdV and $k \geq e^{\Omega(T)}$ for Euler.

**Main theorems:**
- **KdV lower bound:** Any quantum algorithm that simulates the KdV equation to constant accuracy for time $T$ requires $\Omega(T^2)$ copies of the initial state in the worst case.
- **Euler lower bound:** Any quantum algorithm that simulates the incompressible Euler equations to constant accuracy for time $T$ requires $e^{\Omega(T)}$ copies of the initial state in the worst case.

---

## Key results

| Model | Lower bound on copies of initial state |
|---|---|
| KdV equation | $\Omega(T^2)$ |
| Incompressible Euler equations | $e^{\Omega(T)}$ |

**Corollary for quantum advantage:** If a classical algorithm can simulate KdV to accuracy $\varepsilon$ in time $\text{poly}(n, T, 1/\varepsilon)$ (where $n$ is the discretisation size), then the best quantum algorithm requires $\Omega(T^2)$ copies even to *load* the initial state, which is at least quadratically more expensive in the relevant regime. For Euler, any quantum algorithm requires exponentially many copies, making exponential quantum speedup impossible unless classically preparing and re-running the initial state is itself exponentially cheap.

**Interpretation:** These bounds do not rule out all quantum advantage — they constrain what is achievable when the initial state must be prepared anew for each copy. If the initial state can be prepared efficiently and repeatedly, copy complexity may not be the bottleneck. But they significantly constrain the regime in which the optimistic proposals in the literature could work.

---

## Comparison with prior work

**Positive results (quantum algorithms for fluid dynamics):**

| Paper | Model | Claim |
|---|---|---|
| [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]] | General nonlinear ODEs | Exponential speedup (later questioned) |
| [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] | Dissipative nonlinear PDEs (Navier-Stokes-like) | Polynomial speedup under dissipation |
| [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]] | Nonlinear DEs | Improved complexity |
| [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] | Nonlinear DEs | Better dependence on parameters |
| arXiv:2303.16550 (Liu et al.) | Navier-Stokes via lattice Boltzmann | Evidence of exponential speedup |

This paper directly challenges the optimistic narrative from arXiv:2303.16550 and related work by showing that for the Euler equations (which are the inviscid limit of Navier-Stokes), exponential copy costs are unavoidable. The dissipative case (Navier-Stokes with viscosity) is not covered by the lower bounds here, which is a key distinction.

**Lower bounds context:**
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]] — foundational polynomial method for query complexity; this paper adapts the same philosophy to the state-copy setting.
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]] — a parallel paper showing quantum algorithms for TDA don't give exponential speedup; similar spirit of puncturing a proposed quantum advantage.
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]] — Krovi's earlier work on complexity of physical simulation.

---

## Limits / caveats

- The lower bounds apply specifically to the KdV equation and the incompressible Euler equations. They do not cover the Navier–Stokes equations with viscosity, which is where most practical fluid dynamics interest lies and where existing quantum proposals (lattice Boltzmann) focus.
- The lower bound is in terms of **copies of the initial state**, not gate complexity. An algorithm that can efficiently re-prepare the initial state from a classical description might circumvent the lower bound — but then the cost of state preparation becomes the bottleneck anyway.
- Worst-case lower bounds: the Euler lower bound applies in the worst case over initial conditions. For specific structured initial conditions (e.g., laminar flow near equilibrium), the lower bound may not bite.
- The bounds say nothing about the quantum complexity of *approximate* or *statistical* simulation (e.g., computing observables, estimating correlation functions) — these might be easier.
- There may be algorithms that represent fluid states in compressed or implicit forms rather than as explicit quantum states, to which the copy lower bound does not directly apply.

---

## Reusable ideas

- **Copy complexity as a lower bound resource:** Measuring quantum algorithm cost in copies of the initial state (rather than gates) is a robust model-independent lower bound technique. Any algorithm acting on $k$ copies is constrained by the algebraic degree-$k$ structure.
- **Nonlinear sensitivity → copy lower bound:** If a physical evolution has exponential (resp. polynomial-degree $d$) sensitivity in initial conditions, then approximating the output requires $e^{\Omega(T)}$ (resp. $\Omega(T^d)$) copies. This principle should transfer to other nonlinear dynamics with known Lyapunov exponents or polynomial-growth structure.
- **Polynomial method for quantum state algorithms:** The [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|polynomial method]] (originally for query complexity) adapts to the state-copy setting. A quantum algorithm on $k$ copies of $|\psi\rangle$ is a degree-$k$ polynomial in the entries of $\rho = |\psi\rangle\langle\psi|$. This is likely to be useful for other quantum simulation lower bounds.
- **Separating dissipative from conservative cases:** The bounds here apply to the inviscid/conservative Euler equations. Dissipation regularises solutions and reduces sensitivity. The contrast with [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu et al.]] suggests that dissipation is not just a physical detail but a computational one: it is the mechanism that permits quantum speedup.

---

## References within this paper

- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]] — polynomial method foundation
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — the quantum algorithm this paper proves limitations against
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]] — Krovi's quantum algorithms for nonlinear DEs
- [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]] — early nonlinear DE proposal
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — recent positive results for contrast
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]] — complexity of physical simulation (Krovi author)

---

## Cross-links

**Lower bound and complexity limitation papers:**
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]]

**Quantum algorithms for nonlinear PDEs (positive results this paper responds to):**
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]]
- [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]]
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]]
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]
- [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]]
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]]

**Physical simulation complexity:**
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]]
