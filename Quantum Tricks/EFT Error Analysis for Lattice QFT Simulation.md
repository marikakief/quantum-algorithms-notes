# EFT Error Analysis for Lattice QFT Simulation

> **Source:** Jordan, Lee, Preskill, Science 336, 1130 (2012); arXiv:1111.3633
> **Tags:** #trick #effective-field-theory #lattice #error-analysis #renormalization

## What it does
Uses effective field theory (EFT) to systematically classify and bound the errors introduced by lattice discretisation in a quantum simulation of a continuum QFT, replacing ad hoc Taylor-expansion error bounds.

## The trick

Treat the lattice Hamiltonian as the leading term $\mathcal{L}^{(0)}$ of an effective Lagrangian. All operators consistent with the symmetries ($\phi \to -\phi$ for $\phi^4$) appear in the EFT expansion:

$$\mathcal{L}_{\text{eff}} = \mathcal{L}^{(0)} + \frac{c}{6!}\phi^6 + c'\phi\partial^4\phi + \cdots$$

Classify the corrections into three types:
- **Type I** ($\phi^{2n}$, quantum effects): coefficient $\sim \lambda^n a^{2n-D}$
- **Type II** (derivative discretisation): coefficient $\sim a^{2l-2}$
- **Type III** (mixed): coefficient $\sim \lambda^{j+1} a^{2j+2l+2-D}$

Dimensional analysis + power counting gives the lattice-spacing dependence. The dominant error is $O(a^2)$ in $D = 2, 3, 4$ spacetime dimensions. At strong coupling, anomalous dimensions modify the exponents but the scaling remains polynomial in $1/a$.

## When to reach for it

- Any quantum simulation of a lattice-regularised field theory where you need to bound continuum-limit errors
- Choosing the lattice spacing $a$ to achieve target precision $\varepsilon$: set $a \sim \sqrt{\varepsilon}$
- Understanding which irrelevant operators dominate the lattice artifacts and whether improved discretisations (e.g., better finite differences, adding counterterms) would help

## Complexity
This is an analysis technique, not a computational one. It tells you the relationship $\varepsilon = O(a^2)$, which determines how fine the lattice must be. The computational cost then follows from the volume $V = (L/a)^d$ and the simulation time.

## Caveat
EFT arguments give the *scaling* of errors with $a$, not rigorous error bounds with explicit constants. The coefficients depend on the coupling, and at strong coupling they're not calculable perturbatively (though the scaling exponents are constrained by universality). For a theorem-level bound, you'd need constructive field theory methods.

Also: improvement is possible. Adding $\phi^6$ counterterms or using improved lattice derivatives can push the dominant error to $O(a^4)$ — but renormalisation and operator mixing complicate this beyond the standard numerical analysis story.

## Related notes
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]] — the Trotter framework whose locality structure JLP exploit
