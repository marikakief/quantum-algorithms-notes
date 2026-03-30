# Uniform Superposition as Quantum Proof

> **Source:** Watrous, arXiv:cs/0009002 (2000)
> **Tags:** #trick #QMA #quantum-proofs #group-theory #state-as-certificate

## What it does

Uses the uniform superposition $|S\rangle = |S|^{-1/2}\sum_{s \in S}|s\rangle$ over a set $S$ as a quantum certificate (proof) for properties of $S$, in situations where no polynomial-length classical string can certify the same property.

## The trick

The state $|H\rangle$ over a subgroup $H = \langle g_1, \ldots, g_k \rangle$ has two properties that make it useful as a proof of non-membership:

1. **Invariance:** $|H\rangle$ is the unique (up to phase) state invariant under right multiplication by all elements of $H$. This can be tested by [[Invariance Testing via Reversible Random Generation|the invariance test]].
2. **Coset orthogonality:** For $h \notin H$, the coset state $|Hh\rangle$ is orthogonal to $|H\rangle$. This means [[Coset Distinguishing via Controlled Multiplication|controlled multiplication by $h$]] creates a distinguishable state.

The quantum certificate encodes membership information about an exponentially large set in a polynomial number of qubits. A classical certificate can't do this because testing membership in $H$ requires knowing $H$'s structure, and the only known compact classical descriptions (generators) don't directly certify non-membership.

## When to reach for it

- Any problem where the witness is a *set* (or distribution over a set) rather than a single element
- Group-theoretic problems in the black-box setting where subgroup structure is hard to certify classically
- QMA protocols where the natural proof is a quantum state with algebraic structure (uniform superposition, ground state, etc.)
- The idea generalises: [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev's]] QMA-completeness of Local Hamiltonian uses ground states as quantum proofs — same philosophy, different algebraic structure

## Complexity

Preparing $|H\rangle$ from generators requires Babai's nearly-uniform random generation (polynomial time per sample, exponentially close to uniform). The proof itself is $n$ qubits for a group on $n$-bit strings. Verification is polynomial-time with bounded error.

## Caveat

The verifier can check that a given state *is* $|H\rangle$ (via invariance testing), but cannot *prepare* $|H\rangle$ itself in general — that's what makes it a non-trivial proof. For solvable groups, the $|H\rangle$ state can be prepared efficiently by [[Subgroup State Preparation via Subnormal Chain|climbing the subnormal series]] (Watrous 2001), which puts GNM for solvable groups in BQP rather than just QMA.

## Related notes
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
- [[Invariance Testing via Reversible Random Generation]]
- [[Coset Distinguishing via Controlled Multiplication]]
- [[Subgroup State Preparation via Subnormal Chain]]
