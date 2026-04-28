# Copy Lower Bound for Nonlinear Quantum Simulation

> **Source:** Ameri, Carolan, Childs, Krovi, arXiv:2603.12161 (2026)
> **Tags:** #trick #lower-bound #copies #nonlinear-PDEs #polynomial-method #fluid-dynamics #quantum-advantage

## What it does

Proves that any quantum algorithm simulating a nonlinear PDE for time $T$ must use many copies of the initial quantum state, with the required number growing with the nonlinear sensitivity of the dynamics — polynomially ($\Omega(T^2)$) for mildly nonlinear systems like KdV, exponentially ($e^{\Omega(T)}$) for sensitively nonlinear systems like the Euler equations.

## The trick

**Key observation:** Any quantum algorithm that acts on $k$ copies of an unknown initial state $|\psi_0\rangle$ can only compute functions of $\rho_0 = |\psi_0\rangle\langle\psi_0|$ that are **degree-$k$ polynomials** in the entries of $\rho_0$ (or equivalently, multilinear polynomials of degree $k$ in the amplitudes). This is a direct analogue of the [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|polynomial method]] in query complexity, applied to the state-copy setting.

**Lower bound argument:**
1. The solution map $|\psi_0\rangle \mapsto |\psi(T)\rangle$ of a nonlinear PDE is itself a nonlinear function of the initial state.
2. To approximate this map to constant accuracy requires a polynomial approximation of the solution map in $\rho_0$ of sufficiently high degree.
3. The required degree is determined by the growth structure of the nonlinearity:
   - **KdV** (integrable, polynomial nonlinearity): solution map requires degree $\Omega(T^2)$ polynomial approximation → $k \geq \Omega(T^2)$ copies.
   - **Euler** (exponential sensitivity / Lyapunov instability): solution map requires degree $e^{\Omega(T)}$ polynomial approximation → $k \geq e^{\Omega(T)}$ copies.

**Proof sketch:** Construct a family of initial states $\{|\psi_\theta\rangle\}$ parameterised by $\theta$ such that the corresponding time-$T$ solutions are maximally distinguishable, but the initial states are nearly indistinguishable without many copies. Any $k$-copy algorithm must distinguish these; the polynomial degree constraint then forces $k$ to be large.

## When to reach for it

- You want a **model-independent** lower bound on quantum simulation cost (gates, time, copies — anything).
- The dynamics being simulated is **nonlinear** and you have bounds on its sensitivity (Lyapunov exponents, polynomial growth rates).
- You want to rule out exponential quantum speedup for a specific physical system.
- You're arguing that dissipation (not just nonlinearity) is the key ingredient enabling quantum speedup.

## Complexity

| System | Required copies of $|\psi_0\rangle$ | Interpretation |
|---|---|---|
| KdV equation, time $T$ | $\Omega(T^2)$ | Quadratic cost — modest barrier |
| Euler equations, time $T$ | $e^{\Omega(T)}$ | Exponential cost — no polynomial speedup possible |

## Caveat

- The lower bound is in **copy complexity**, not gate complexity. If the initial state can be prepared cheaply from a classical description and re-prepared freely, the copy cost is not directly a gate cost. However, state preparation itself often requires $O(\text{poly}(n))$ gates, making repeated preparation expensive.
- Applies to **worst-case** initial conditions. For structured or near-equilibrium initial states, the sensitivity may be lower and the lower bound may not apply.
- Does **not** cover the Navier–Stokes equations with viscosity — dissipation changes the sensitivity and leaves room for quantum speedup (as in [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]]).
- Statistical observables (e.g., energy spectra, correlation functions) might be computable with fewer copies than needed to simulate the full state.

## Related notes

- [[Quantum Lower Bounds for Simulating Fluid Dynamics (Ameri-Carolan-Childs-Krovi 2026) — Paper Notes]]
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]]
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]]
- [[Adversary Method for Quantum Query Lower Bounds]]
