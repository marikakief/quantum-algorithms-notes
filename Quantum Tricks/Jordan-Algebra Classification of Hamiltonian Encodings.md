# Jordan-Algebra Classification of Hamiltonian Encodings

> **Source:** Toby Cubitt, Ashley Montanaro, Stephen Piddock, arXiv:1701.05182
> **Tags:** #trick #encoding #Hamiltonian-simulation #Jordan-algebra

## What it does

Characterises every valid spectrum-preserving linear Hamiltonian encoding: it must be a unitary embedding of copies of $H$ and its complex conjugate $\overline{H}$.

## The trick

Suppose an encoding map $E$ on Hermitian matrices preserves three basic things:

1. Hermiticity,
2. the full spectrum,
3. linear combinations term by term.

Then $E$ is forced to be a Jordan homomorphism on Hermitian matrices. By Jordan-to-$*$-homomorphism structure theorems, it extends uniquely to a matrix-algebra homomorphism of the form
$$
E(H) = U\bigl(H^{\oplus p} \oplus \overline{H}^{\oplus q}\bigr)U^\dagger
$$
for some unitary $U$ and integers $p,q\ge 0$ with $p+q\ge 1$.

So if you want an analogue simulation that really preserves what a Hamiltonian means operationally, you do **not** get to invent an arbitrary nonlinear encoding. The algebra pins it down.

This becomes the backbone for rigorous definitions of Hamiltonian simulation, local encodings, observable mappings, Gibbs-state mappings, and noise mappings in [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]].

## When to reach for it

- When defining a rigorous notion of analogue [[Hamiltonian simulation]]
- When you need to prove that a proposed encoding is the only possible one up to unitary change of basis
- When relating simulation of complex Hamiltonians to simulation of real ones via a direct-sum construction
- When checking whether a low-energy gadget really preserves full physics rather than only the ground energy

## Complexity

Conceptual rather than algorithmic. Once the encoding axioms are in place, the classification is exact and incurs only constant-factor ancilla overhead through the multiplicities $p$ and $q$.

## Caveat

This classifies **exact linear encodings**. Approximate gadget simulations still need a separate perturbative error analysis to show that the implemented low-energy subspace is close to such an encoding.

## Related notes

- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]]
- [[Complex-to-Real Hamiltonian Encoding]]
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]]
