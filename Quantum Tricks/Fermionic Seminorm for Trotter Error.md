# Fermionic Seminorm for Trotter Error

> **Source:** Yuan Su, Hsin-Yuan Huang, Earl T. Campbell, arXiv:2012.09194  
> **Tags:** #trick #fermionic #trotter #hamiltonian-simulation #electronic-structure

## What it does

Replaces spectral-norm Trotter error analysis with a seminorm restricted to the $\eta$-electron subspace, removing spurious dependence on the total Hilbert space dimension.

## The trick

For a number-preserving operator $X$ (such as the [[Product Formulas|Trotter]] error operator for an electronic Hamiltonian), define:

$$\|X\|_\eta := \max_{|\psi_\eta\rangle, |\phi_\eta\rangle} |\langle \phi_\eta | X | \psi_\eta \rangle|$$

where $|\psi_\eta\rangle, |\phi_\eta\rangle$ live in the $\eta$-electron manifold. This equals the projected spectral norm $\|X\Pi_\eta\| = \|\Pi_\eta X \Pi_\eta\|$.

The seminorm satisfies all the standard properties (submultiplicativity, unitary invariance within the number-preserving subalgebra) and is related to the expectation by:

$$\max_{|\psi_\eta\rangle} |\langle \psi_\eta | X | \psi_\eta \rangle| \leq \|X\|_\eta \leq 2 \max_{|\psi_\eta\rangle} |\langle \psi_\eta | X | \psi_\eta \rangle|$$

The factor of 2 comes from a polarisation identity and is tight.

Plug this into the [[Trotter Commutator-Scaling Bound|commutator representation of Trotter error]]: instead of bounding nested commutators in spectral norm, bound them in the fermionic seminorm. For an electronic Hamiltonian $H = T + V$ with $T = \sum \tau_{j,k} A_j^\dagger A_k$ and $V = \sum \nu_{l,m} N_l N_m$, the nested commutators $[H_{\gamma_{p+1}}, \ldots [H_{\gamma_2}, H_{\gamma_1}]]$ have seminorms controlled by $\eta$ (not $n$), because the fermionic anticommutation relations constrain which matrix elements contribute within the $\eta$-particle sector.

## When to reach for it

- Simulating fermionic systems where $\eta \ll n$ (dilute regime — many more orbitals than electrons)
- Any number-preserving Hamiltonian where the physical subspace is much smaller than the full Fock space
- Comparing Trotter costs for electronic structure: spectral-norm bounds can be misleading by factors of $n^p$ or more

## Complexity

No additional computational cost — this is purely an analysis tool. The simulation algorithm is unchanged; only the error bound improves.

## Caveat

Only applies to number-preserving operators and states within a fixed particle-number sector. If the simulation involves particle creation/annihilation (e.g., QFT simulations), or if the initial state is a superposition over different particle numbers, the seminorm doesn't directly apply.

## Related notes

- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Trotter Commutator-Scaling Bound]]
- [[Nuclear Charge Scaling of Trotter Error]]
- [[Recursive Operator Cauchy-Schwarz for Fermionic Bounds]]
