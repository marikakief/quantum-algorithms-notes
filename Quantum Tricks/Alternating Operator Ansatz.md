
> **Source:** Farhi, Goldstone & Gutmann, arXiv:1411.4028 (2014); generalised by Hadfield et al. (2019)
> **Tags:** #trick #QAOA #variational #ansatz #combinatorial-optimization

## What it does

Constructs a parametrised quantum state by alternating between a phase-separation operator (encoding the problem) and a mixing operator (enabling exploration), repeated $p$ times with tuneable angles.

## The trick

Given an objective $C$ diagonal in the computational basis and a mixing Hamiltonian $B$ (typically $\sum_j \sigma_j^x$), the ansatz state is:

$$|\boldsymbol{\gamma}, \boldsymbol{\beta}\rangle = U(B, \beta_p) U(C, \gamma_p) \cdots U(B, \beta_1) U(C, \gamma_1) |s\rangle$$

where $U(C, \gamma) = e^{-i\gamma C}$ and $U(B, \beta) = e^{-i\beta B}$, starting from the uniform superposition $|s\rangle$.

The phase operator imprints the objective's structure into amplitudes. The mixer operator redistributes amplitude. Alternating them creates interference patterns that concentrate amplitude on high-value solutions.

Replacing $B$ with problem-specific mixers (e.g., $XY$-mixers for Hamming-weight constraints, ring mixers for routing problems) gives constrained variants.

## When to reach for it

- Combinatorial optimisation on near-term hardware: low-depth circuits with classical outer-loop optimisation
- Problems with efficiently computable local cost functions (each clause involves few qubits)
- When the adiabatic algorithm works well conceptually but you want a discrete-gate version — QAOA is Trotterised [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|adiabatic evolution]]
- When you need a structured ansatz (as opposed to hardware-efficient random circuits) for [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|variational algorithms]]

## Complexity

Circuit depth: $O(p \cdot m)$ where $m$ is the number of clauses. Classical parameter optimisation: $2p$ angles to tune. For fixed $p$ on bounded-degree problems, the angles can be found classically via the [[Locality-Based Classical Preprocessing for Variational Circuits|subgraph decomposition]].

## Caveat

No proven quantum advantage for any specific problem at any fixed $p$. Classical algorithms match or beat QAOA at small $p$ for the problems analysed so far (MaxCut, Max-E3LIN2). The parameter landscape suffers from barren plateaus at large $n$ and may have many local minima. Whether large $p$ helps in practice is an open question.

## Related notes

- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]] — the original paper
- [[Locality-Based Classical Preprocessing for Variational Circuits]] — the subgraph decomposition for angle selection
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]] — QAOA as discretised adiabatic evolution
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — the parallel variational approach
- [[Standard Amplitude Amplification]] — Grover's iteration as a simple two-operator alternation
