# Complex-to-Real Hamiltonian Encoding

> **Source:** Toby Cubitt, Ashley Montanaro, Stephen Piddock, arXiv:1701.05182
> **Tags:** #trick #encoding #real-Hamiltonian #Hamiltonian-simulation

## What it does

Encodes an arbitrary complex Hamiltonian into a real Hamiltonian, so any simulator that can only realise real interactions can still simulate fully general quantum systems.

## The trick

The clean algebraic form is
$$
\varphi(H) = U(H \oplus \overline{H})U^\dagger,
$$
where $U$ is a fixed unitary and $\overline{H}$ is the complex conjugate of $H$.

Equivalently, one can view the construction as adjoining an ancilla that tracks whether the encoded system is in the $H$ block or the $\overline{H}$ block. In a suitable basis, the encoded Hamiltonian becomes manifestly real.

In [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]], this is the step that upgrades universality proofs for real interactions such as Heisenberg-type and XY-type couplings into universality for arbitrary complex target Hamiltonians.

The important point is locality: the paper does not stop at the abstract direct sum. It shows how to make the encoding local by distributing the extra ancilla structure across the simulated subsystems.

## When to reach for it

- When your available interaction set only generates real Hamiltonians but your target problem is fully complex
- When proving universality of a real-valued Hamiltonian family
- When translating between real and complex formulations of many-body simulation

## Complexity

Constant-factor overhead in local dimension, plus whatever perturbative overhead is needed to enforce the encoded ancilla sector in a local simulator Hamiltonian.

## Caveat

The direct-sum formula is easy; making it a **local** encoded simulation is the nontrivial part. Also, the trick preserves the target physics through an encoding, not by identifying the real Hamiltonian with the original one term by term.

## Related notes

- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]]
- [[Jordan-Algebra Classification of Hamiltonian Encodings]]
- [[Symmetry-Breaking Encoding via Degenerate Ground Spaces]]
