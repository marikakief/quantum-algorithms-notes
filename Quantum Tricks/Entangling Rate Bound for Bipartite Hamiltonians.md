# Entangling Rate Bound for Bipartite Hamiltonians

> **Source:** Bravyi, Hastings, Verstraete, arXiv:quant-ph/0603121; Childs, Leung, Verstraete, Vidal (2003); Childs, Leung, Vidal (2004)
> **Tags:** #trick #entanglement #area-law #Hamiltonian-simulation #information-theory

## What it does
Bounds the maximum rate at which entanglement entropy can grow across a bipartition under local Hamiltonian evolution, yielding an area law for entropy generation in time.

## The trick

For a Hamiltonian coupling regions $A$ and $B$ via $P$ boundary terms:

$$H(t) = H_A(t) + H_B(t) + \sum_{k=1}^P r_k(t) \, J_A^k \otimes J_B^k$$

with $\|J_A^k\|, \|J_B^k\| \leq 1$, the entanglement entropy rate is bounded by:

$$\frac{dS(\rho_A)}{dt} \leq c^* \sum_{k=1}^P |r_k(t)|$$

where $c^* = 2\max_{0 \leq x \leq 1} \sqrt{x(1-x)} \log\frac{x}{1-x} \approx 1.9$ is the maximum entangling rate of a single product interaction — independent of the Hilbert space dimensions of $A$ and $B$.

The bound comes from Childs-Leung-Verstraete-Vidal (2003), applied via Trotter approximation: at each infinitesimal time step, only one $J_A^k \otimes J_B^k$ term acts, contributing at most $c^* |r_k|$ to the entropy rate.

**Integrated bound:** For bounded interactions $|r_k(t)| \leq g$:

$$S(\rho_A(t)) - S(\rho_A(0)) \leq c^* g P t$$

Entropy growth is proportional to **perimeter $\times$ time**, not volume.

**Consequence:** If you start with a state satisfying an area law (entropy of a block $\propto$ surface area), and evolve for finite time, the state still satisfies an area law. This is why MPS/tensor network methods remain accurate for finite-time dynamics — the entanglement doesn't explode.

## When to reach for it

- Justifying why [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal-style MPS simulation]] works for finite-time evolution of area-law initial states
- Bounding the bond dimension growth in TEBD/DMRG time-evolution simulations
- Arguing that ground states within the same phase (connected by finite-time quasi-adiabatic evolution) all satisfy area laws if one does
- Estimating the classical resources needed to track quantum dynamics

## Complexity

The bound is on the *physics*, not on a specific algorithm. But it directly constrains:
- **MPS bond dimension:** After time $t$, bond dimension grows at most as $e^{c^* g P t}$, where $P$ is the cut perimeter
- **Classical simulation cost:** Polynomial in system size for $t = O(1)$ starting from area-law states

## Caveat

- Only applies to **real-time** (unitary) evolution. Imaginary-time evolution can generate unbounded entanglement per unit time.
- The constant $c^* \approx 1.9$ is tight for specific interactions but is a loose bound for typical physical Hamiltonians.
- For long times $t = \Omega(L)$, the bound allows volume-law entanglement — it's an area law *in time*, not an absolute area law.
- Does not apply to non-local Hamiltonians or measurement-based protocols (which can create long-range entanglement in $O(1)$ time if classical communication is free).

## Related notes
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes]]
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]]
- [[Light-Cone Truncation for Operator Approximation]]
