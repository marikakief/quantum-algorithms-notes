# Anyonic Braiding as Fault-Tolerant Gates

> **Source:** Kitaev, arXiv:quant-ph/9707021 (1997/2003)
> **Tags:** #trick #topological #anyons #fault-tolerance #braiding #non-abelian

## What it does

Implements quantum gates on topologically protected qubits by physically moving non-abelian anyons around each other. The resulting unitary depends only on the *topology* of the braid (which particle went around which), not on the precise trajectory — giving intrinsic fault tolerance.

## The trick

In a system with non-abelian anyons (e.g., from the quantum double $D(G)$ of a non-abelian group $G$):

1. **Create particle pairs** from the vacuum (ground state) using [[Ribbon Operators for Particle Creation|ribbon operators]].
2. **Braid particles:** Moving particle $i$ around particle $j$ applies a unitary $R_{ij}$ on the protected space $\mathcal{M}_{d_1, \ldots, d_n}$.
3. **Topological invariance:** Small perturbations to the braiding path don't change the gate, because the unitary depends only on the topological class of the braid. Errors in the trajectory are automatically corrected as long as no anyon passes through another.
4. **Measure:** Fuse pairs of particles and observe the resulting particle type. This projects the protected space, implementing a measurement.

**Why it's fault-tolerant:** The gate is encoded in the topology of the particle trajectories (a braid), not in precise control parameters (pulse timing, amplitude, etc.). Local noise that doesn't change the braid topology doesn't change the gate. The error rate scales as $\exp(-aL)$ where $L$ is the minimum distance between particles.

For abelian anyons ($G = \mathbb{Z}_2$), braiding only gives phases ($\pm 1$), producing just $\sigma^x$ and $\sigma^z$ — not universal. For non-abelian anyons (e.g., $G = S_3$ or larger), braiding gives multi-dimensional unitary representations of the braid group, potentially universal.

## When to reach for it

- Designing fault-tolerant quantum computers based on topological phases of matter
- Understanding the computational power of specific anyon models (which groups $G$ give universal braiding?)
- Connecting knot invariants to quantum computation: the Jones polynomial at roots of unity is computed by braiding specific anyons ([[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL 2006]])
- As a conceptual framework: "computation as topology" rather than "computation as dynamics"

## Complexity

The number of elementary braiding operations for a gate scales with the circuit complexity of the gate in the standard model (up to constant factors depending on the anyon model). The Solovay-Kitaev theorem applies if the braiding generates a dense subgroup of $\text{SU}(d)$ on the protected space.

## Caveat

**Universality is not automatic.** Whether braiding alone gives universal gates depends on the specific anyon model:
- Fibonacci anyons ($SU(2)_3$): universal via braiding alone
- Ising anyons ($SU(2)_2$): NOT universal by braiding alone; need supplementary operations
- Quantum double $D(G)$: depends on $G$

**Physical realisation:** Non-abelian anyons are hard to create experimentally. Fractional quantum Hall state at $\nu = 5/2$ is the primary candidate, but evidence remains inconclusive. Microsoft's topological qubit programme pursues Majorana-based approaches.

## Related notes
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]
- [[Topological Degeneracy as Quantum Memory]]
- [[Ribbon Operators for Particle Creation]]
- [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
