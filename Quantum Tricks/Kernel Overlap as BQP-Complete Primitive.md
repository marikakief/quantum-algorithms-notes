# Kernel Overlap as BQP-Complete Primitive

> **Source:** Gyurik, Schmidhuber, King, Dunjko, Hayakawa, arXiv:2410.21258 (2024)
> **Tags:** #trick #complexity-theory #BQP #guided-Hamiltonian #kernel

## What it does

Identifies "Kernel Overlap" — deciding whether a given state has large overlap with the kernel of a sparse Hamiltonian — as a natural BQP$_1$-complete computational primitive, distinct from the standard [[Guided Sparse Hamiltonian Framework|guided sparse Hamiltonian problem]].

## The trick

**Kernel Overlap problem:** Given a log-local Hamiltonian $H = \sum_{i=1}^m H_i$ with spectral gap $\gamma(H) \geq 1/\text{poly}(n)$, and a succinct description of a state $|\psi\rangle$, decide whether:
- $\|\operatorname{proj}_{\ker(H)}(|\psi\rangle)\| \geq 1/\text{poly}(n)$, or
- $\|\operatorname{proj}_{\ker(H)}(|\psi\rangle)\| < 1/\exp(n)$

**Containment in BQP:** Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|QPE]] on $e^{iH}$ with $|\psi\rangle$ in the eigenvector register, precision $\sim \gamma(H)$. Repeat $O(\text{poly}(n))$ times. Output 1 if any zero eigenvalue is observed.

**BQP$_1$-hardness:** Reduce from any BQP$_1$ circuit using the [[History-State Encoding with Unary Clock|circuit-to-Hamiltonian]] construction + [[Pre-History State as Guided Sparse Hamiltonian Input|pre-history state]] as the input state.

The distinction from the guided sparse Hamiltonian problem: in the guided version, the state is promised to have large overlap with the ground state in *both* YES and NO cases, and the task is to distinguish ground-state energy above/below a threshold. In Kernel Overlap, the state might have *zero* overlap with the kernel (NO case), and the task is to detect any overlap at all. The hardness comes from a fundamentally different source.

**Low-Energy Overlap** (the BQP-complete generalisation): replace "kernel" with "subspace of eigenvalues $\leq \eta$". This removes the perfect-completeness requirement and captures standard BQP.

## When to reach for it

- Problems naturally phrased as "does this state overlap with the zero-energy subspace?" — including harmonic persistence in TDA, quantum satisfiability witnesses, and spectral gap certification
- As a reduction target for BQP$_1$-hardness proofs: the Kernel Overlap structure (overlap with kernel, not energy estimation) can be more natural than the standard local Hamiltonian formulation
- The Low-Energy Overlap variant works as a BQP-completeness target when perfect completeness isn't available

## Complexity

The containment side is just QPE with $O(\text{poly}(n))$ repetitions. The total cost is $O(\text{poly}(n) / \gamma(H))$ for simulation + phase estimation.

## Caveat

The spectral gap $\gamma(H)$ must be at least $1/\text{poly}(n)$ — this is a promise, not something the algorithm verifies. Without this promise, distinguishing zero eigenvalues from exponentially small ones is impossible. Also, the state $|\psi\rangle$ must be efficiently preparable from its succinct description.

For the TDA application, the spectral gap comes from the King-Kohler analysis of the combinatorial Laplacian of the gadget-augmented clique complex — a nontrivial structural result.

## Related notes
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]]
- [[Guided Sparse Hamiltonian Framework]]
- [[Pre-History State as Guided Sparse Hamiltonian Input]]
- [[History-State Encoding with Unary Clock]]
- [[Kernel Dimension Estimation via QPE on Mixed States]]
