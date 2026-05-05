# Band-Slicing Reduction for 2D Shallow-State Learning

> **Source:** Huang, Liu, Broughton, Kim, Anshu, Landau, McClean, arXiv:2401.10095
> **Tags:** #trick #state-learning #shallow-circuits #2D-lattices #disentangling

## What it does

Reduces learning a 2D shallow-circuit state to a sequence of effectively 1D learning problems by disentangling alternating strips of the lattice.

## The trick

Partition the 2D lattice into vertical bands. Learn local disentangling unitaries that decouple every other band from the rest, leaving those qubits close to $|0\rangle$ and collapsing the residual problem onto narrower strips. The remaining pieces inherit enough 1D structure that dynamic-programming or 1D structured-tomography methods can finish the job.

Conceptually:

1. disentangle alternating bands,
2. reduce the geometry from 2D to several coupled 1D slices,
3. learn each slice,
4. invert the disentangling circuit.

The trick is not that 2D magically becomes easy; it is that shallow depth keeps correlations confined enough for this geometric peeling to work.

## When to reach for it

- The target state is prepared by a shallow circuit on a 2D lattice.
- Purely 1D tomography tools are available but 2D looks too large.
- You can exploit finite correlation length and locality.

## Complexity

Polynomial in system size for fixed depth, but with large exponents and strong depth dependence. This is a structural reduction, not a lightweight practical primitive.

## Caveat

The method is tightly tied to 2D geometry and shallow depth. It does not obviously extend to 3D or to circuits whose correlations span wide bands.

## Related notes

- [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]]
- [[Learning quantum states prepared by shallow circuits in polynomial time (Landau-Liu 2024) — Paper Notes]]
- [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes]]
- [[Sequential Disentanglement for MPS Tomography]]
