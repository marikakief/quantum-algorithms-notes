---
aliases:
  - Lieb-Robinson
  - Lieb-Robinson bounds
---

# Lieb-Robinson Bound

**Concept hub** — the finite speed of information propagation in local quantum systems.

---

## What it is

For a geometrically local Hamiltonian $H = \sum_{X} h_X$ on a lattice, the commutator between observables $A$ at site $x$ and $B$ at site $y$ under time evolution satisfies:

$$\|[A(t), B]\| \leq C \|A\|\|B\| \, e^{v_{\mathrm{LR}}|t| - \mu \, d(x,y)}$$

where $v_{\mathrm{LR}}$ is the **Lieb-Robinson velocity** (a finite effective "speed of light") and $\mu > 0$ controls the exponential decay outside the light cone.

The bound means that for $|t| \ll d(x,y)/v_{\mathrm{LR}}$, the effect of $B$ on the time-evolved $A(t)$ is exponentially small. Information effectively travels no faster than $v_{\mathrm{LR}}$.

---

## Why it matters for quantum simulation

The Lieb-Robinson bound is the key reason geometrically local Hamiltonians are easier to simulate than general sparse Hamiltonians:

1. **Lattice decomposition:** The time-evolution unitary $e^{-iHt}$ for short $t$ can be approximated by a product of unitaries acting on bounded regions — terms far apart don't interact yet. See [[Lieb-Robinson Decomposition for Lattice Simulation]].

2. **Gate complexity lower bounds:** The light cone sets a lower bound on circuit depth for simulating local dynamics. Any circuit simulating $e^{-iHt}$ on an $n$-site lattice needs $\Omega(nt)$ gates, because the light cone tiles the lattice into $\Theta(n)$ independent causal regions. See [[Circuit-Size Hierarchy for Gate-Complexity Lower Bounds]].

3. **Commutator structure:** Tighter versions of the bound that track nested commutators give improved error estimates for product formulas on local systems. See [[Commutator-Sensitive Lieb-Robinson Bound]].

---

## Key results in this vault

- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes|Haah-Hastings-Kothari-Low (2021)]] — $\tilde{O}(n T)$ gate count for $D$-dimensional lattice simulation, matching the lower bound (optimal up to log factors). The core algorithm builds on Lieb-Robinson decomposition.

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — commutator-based Trotter error bounds exploit the locality structure that Lieb-Robinson enables.

---

## Trick cards

- [[Lieb-Robinson Decomposition for Lattice Simulation]] — factoring $e^{-iHt}$ into local blocks via the light cone
- [[Commutator-Sensitive Lieb-Robinson Bound]] — tighter version tracking nested commutators
- [[Circuit-Size Hierarchy for Gate-Complexity Lower Bounds]] — using the light cone to prove gate-count lower bounds
- [[Local-Observable-First Trotterization]] — ordering Trotter terms to respect locality

---

## Relationship to other concepts

- **[[Product Formula (Trotter-Suzuki)|Product formulas]]** on local Hamiltonians benefit from Lieb-Robinson: error terms involve commutators of spatially separated operators, which are exponentially small.
- **[[Hamiltonian simulation]]** — the Lieb-Robinson bound separates "local" from "general" simulation: local systems admit $O(nT)$ scaling while general sparse Hamiltonians cost $O(d^2 \|H\| T)$ or worse.
- The bound is purely a property of the Hamiltonian's geometry — it holds for any local system regardless of the simulation method.
