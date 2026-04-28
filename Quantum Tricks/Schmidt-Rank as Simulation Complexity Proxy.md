# Schmidt-Rank as Simulation Complexity Proxy

> **Source:** Zhao, Zhou, Childs, arXiv:2406.02379
> **Tags:** #trick #hamiltonian-simulation #entanglement #classical-vs-quantum

## What it does

This card is best read as a caution: the tempting summary “Trotter error decays as inverse Schmidt rank” is too crude for this paper. The actual control parameter is **local reduced-state mixedness on the supports of the leading error operators**.

## The trick

The paper does support a real structural contrast:

- classical tensor-network simulation becomes hard when entanglement grows,
- [[Product Formulas|product-formula]] simulation can become *easier* when the relevant local reduced density matrices become close to maximally mixed.

But that improvement is not captured by a single global Schmidt rank. What matters is whether the small subsystems touched by $E_j^\dagger E_{j'}$ look random enough that

$$
\operatorname{Tr}|\rho_{j,j'}-I/d_{\rho_{j,j'}}|
$$

or equivalently

$$
\log d_{\rho_{j,j'}} - S(\rho_{j,j'})
$$

is small.

So the reusable lesson is weaker and more accurate than the old version of this card: local entanglement that destroys tensor-network compressibility can simultaneously drive Trotter error toward the Frobenius / average-case regime.

## When to reach for it

- When contrasting classical MPS intuition with digital quantum simulation intuition
- When you want to explain why thermalized states can be classically harder yet easier for Trotter error analysis
- When checking whether a global entanglement proxy is too blunt for a local-error problem

## Complexity

No direct complexity formula here. It is a conceptual guide for which entanglement data matter.

## Caveat

- Global Schmidt rank can be misleading. A state may be highly entangled globally yet fail to help if the relevant local RDMs are not close to maximally mixed.
- Conversely, local approximate uniformity on small supports can already be enough, even without an especially dramatic global statement.

## Related notes

- [[Entanglement Accelerates Quantum Simulation (Zhao-Zhou-Childs 2025) — Paper Notes]]
- [[Entanglement-Entropy-Dependent Trotter Error Bound]]
- [[k-Uniformity Convergence to Average-Case Error]]
- [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]]
