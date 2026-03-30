# Topological Degeneracy as Quantum Memory

> **Source:** Kitaev, arXiv:quant-ph/9707021 (1997/2003)
> **Tags:** #trick #topological #toric-code #error-correction #quantum-memory

## What it does

Stores quantum information in the degenerate ground state of a topologically ordered Hamiltonian, where the degeneracy is determined by the topology of the surface (not by any local property) and is exponentially stable against local perturbations.

## The trick

For a stabiliser Hamiltonian $H_0 = -\sum_s A_s - \sum_p B_p$ on a 2D lattice on a surface of genus $g$:

- The ground space has dimension $4^g$ (for the $\mathbb{Z}_2$ toric code) or more generally $|Z(D(G))|^g$ for the quantum double of group $G$.
- **No local operator can distinguish ground states:** any operator supported on a contractible region acts trivially on the ground space (it's a product of stabilisers).
- **Perturbation stability:** under local perturbation $V$ with $\|V\| < \Delta E$, the ground state splitting is $\sim \exp(-aL)$ where $L$ is the smallest dimension of the lattice. The argument: connecting distinct ground states requires a product of operators along a non-contractible path, which takes $\lceil k/2 \rceil$ orders of perturbation theory.

The information is stored in the *topology* of the state — which non-contractible cycles have $\sigma^z$ or $\sigma^x$ products acting on them — rather than in any local degree of freedom.

## When to reach for it

- Designing quantum error-correcting codes with local checks (the toric/surface code family)
- Understanding topological phases of matter — the ground state degeneracy on different surfaces is a signature of topological order
- Arguments about stability of quantum information against thermal noise (requires a gap, plus low-temperature cooling)
- Any construction where you want quantum information that is inherently non-local and protected by an energy gap

## Complexity

Encoding: 2 logical qubits in $2k^2$ physical qubits for a $k \times k$ toric code. Code distance $k$. Gate set from abelian anyons: only $\sigma^x, \sigma^z$ (not universal). For universal gates, need non-abelian anyons from $D(G)$ with appropriate $G$.

## Caveat

The protection is against *local* perturbations only. A perturbation strong enough to close the gap (causing a phase transition) destroys the topological order and the degeneracy. Also, the code rate $2/n \to 0$ — the toric code is not an asymptotically "good" code, though it corrects $O(n)$ random errors at constant rate below threshold.

Thermal protection at finite temperature is subtle: in 2D, the memory lifetime is $O(L)$ (not exponential in $L$), because thermal anyons can propagate across the system in time $O(L)$. True self-correcting quantum memory may require higher dimensions or more exotic models.

## Related notes
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]
- [[Anyonic Braiding as Fault-Tolerant Gates]]
- [[Ribbon Operators for Particle Creation]]
- [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]]
