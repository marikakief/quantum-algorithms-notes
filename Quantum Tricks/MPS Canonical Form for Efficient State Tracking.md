# MPS Canonical Form for Efficient State Tracking

> **Source:** Vidal, arXiv:quant-ph/0301063
> **Tags:** #trick #tensor-networks #MPS #classical-simulation #Schmidt-rank

## What it does
Represents an $n$-qubit pure state using $O(n \cdot \chi^2)$ parameters instead of $2^n$, where $\chi$ is the maximum Schmidt rank across bipartitions.

## The trick
Decompose the state $|\Psi\rangle$ into a chain of local tensors and Schmidt coefficient vectors:

$$c_{i_1 \cdots i_n} = \sum_{\alpha_1, \ldots, \alpha_{n-1}} \Gamma^{[1]i_1}_{\alpha_1} \lambda^{[1]}_{\alpha_1} \Gamma^{[2]i_2}_{\alpha_1 \alpha_2} \lambda^{[2]}_{\alpha_2} \cdots \Gamma^{[n]i_n}_{\alpha_{n-1}}$$

Each tensor $\Gamma^{[l]}$ has dimension $2 \times \chi \times \chi$ (physical index $\times$ left bond $\times$ right bond). The vectors $\lambda^{[l]}$ store the Schmidt coefficients for the cut $[1\cdots l]:[(l+1)\cdots n]$.

Construction: iteratively compute Schmidt decompositions from left to right, peeling off one qubit at a time. At step $l$, expand the Schmidt vector for the right part in the local basis of qubit $l+1$, then re-Schmidt-decompose.

The canonical form means every $\lambda^{[l]}$ is already the Schmidt spectrum for its cut — no additional computation needed to extract entanglement information.

## When to reach for it
- Simulating quantum circuits where entanglement stays bounded (e.g., 1D non-critical spin chains, shallow circuits, QAOA at low depth)
- Any setting where you need to track a quantum state through a sequence of local operations and the state doesn't become maximally entangled
- Building classical benchmarks for quantum algorithms: if MPS simulation works, the quantum algorithm isn't giving you an exponential advantage

## Complexity
- Storage: $O(n\chi^2)$ parameters
- Single-qubit gate update: $O(\chi^2)$
- Two-qubit gate on adjacent sites: $O(\chi^3)$ via [[Local Gate Update on MPS via Schmidt Redecomposition]]
- Non-adjacent two-qubit gate: $O(n\chi^3)$ after SWAP routing

## Caveat
The bond dimension $\chi$ can grow exponentially with circuit depth for general circuits — this is efficient only when entanglement stays bounded. The decomposition also depends on the qubit ordering: a bad ordering can give artificially large $\chi$ even for states that are tractable under a better ordering. Finding the optimal ordering is itself a hard problem in general (related to treewidth).

## Related notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]] — TEBD algorithm, the practical simulation method built on this representation
- [[Local Gate Update on MPS via Schmidt Redecomposition]]
- [[Entanglement as Simulation Complexity Measure]]
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]] — PEPS, the 2D generalisation
