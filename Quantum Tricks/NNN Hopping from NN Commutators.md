# NNN Hopping from NN Commutators

> **Source:** Chen, Childs, Hafezi, Jiang, Kim, Xu, arXiv:2111.12177
> **Tags:** #trick #product-formula #commutator #lattice-simulation #lattice-gauge-theory

## What it does
Generates next-nearest-neighbour (NNN) hopping interactions from commutators of nearest-neighbour (NN) hopping terms, without requiring explicit multi-qubit gate decompositions.

## The trick
For fermionic (or bosonic) hopping operators on a chain, partition NN bonds into even ($H_0$) and odd ($H_1$) sets. Their commutator produces NNN hopping:

$$[c_i^\dagger c_j + \text{h.c.}, \, c_j^\dagger c_k + \text{h.c.}] = c_i^\dagger c_k - c_k^\dagger c_i.$$

More concretely, with $H_0 = \sum_j (c_{2j}^\dagger c_{2j+1} + \text{h.c.})$ and $H_1 = \sum_j (c_{2j+1}^\dagger c_{2j+2} + \text{h.c.})$:

$$H_{\text{eff}} = t(H_0 + H_1) + it^2[H_0, H_1]$$

where the NNN terms $c_j^\dagger c_{j+2}$ appear with alternating $\pm i$ phases, corresponding to $\pi/2$ flux per triangle.

In qubit language (via Jordan-Wigner), the NNN hopping $c_j^\dagger c_{j+2}$ maps to the 3-qubit interaction $\sigma_j^+ \sigma_{j+1}^z \sigma_{j+2}^-$.

On 2D lattices, the same approach generates NNN hopping with phase patterns suitable for the Kapit-Mueller fractional quantum Hall model: split NN hopping into four groups by bond direction and sublattice, then $-i[H_1 - H_2, H_3 - H_4]$ produces the correct NNN terms with magnetic flux phases.

## When to reach for it
- Simulating lattice gauge theories where multi-site interactions arise from commutators of simpler terms
- Fractional quantum Hall models on lattices (Kapit-Mueller type) where band-flattening requires NNN hopping with specific flux patterns
- Any situation where the native gate set is restricted to 2-local (NN) but the target Hamiltonian has 3-local or NNN terms

## Complexity
$3nL$ gates for an $L$-site chain with $n$ time steps — same scaling as NN-only [[Product Formulas|Trotterization]]. The [[Free Sum-and-Commutator Simulation]] trick means the NN terms are included at no extra cost.

## Caveat
The NNN terms come with fixed relative phases ($\pm i$ alternating in 1D) determined by the commutator structure. You can tune the NNN amplitude via the commutator coefficient $\beta$, but the phase pattern is locked. Not all desired NNN Hamiltonians can be reached this way.

## Related notes
- [[Efficient Product Formulas for Commutators and Applications to Quantum Simulation (Chen-Childs-Hafezi-Jiang-Kim-Xu 2022) — Paper Notes]]
- [[Free Sum-and-Commutator Simulation]]
- [[Anticommutator Simulation via Qubit Dilation]]
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]
