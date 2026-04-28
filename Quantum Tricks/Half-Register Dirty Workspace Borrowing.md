# Half-Register Dirty Workspace Borrowing

> **Source:** Gidney, arXiv:2507.23079
> **Tags:** #trick #quantum-arithmetic #dirty-qubits #circuit-primitive #ancilla-management

## What it does

Eliminates the need for external dirty ancillae by splitting the target register in half and using each half as dirty workspace for the other.

## The trick

Many quantum arithmetic circuits need $O(n)$ dirty ancillae as scratch space (e.g., the carry-xor circuit from [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner et al.]]). When the only large register available is the target register itself, you can self-supply the workspace:

1. **Process the bottom half** of the target register first. The top half is untouched and available as dirty workspace — but don't use it yet if a later step needs it clean.
2. **Process the top half**, borrowing the (now-modified) bottom half as dirty workspace. The bottom half is in a known-but-quantum state, which is fine — dirty ancilla circuits restore borrowed qubits to their original state.
3. **Complete deferred work on the bottom half**, now borrowing the (finished) top half as dirty workspace.

The sequencing matters: each half must be in a state where it can be borrowed (i.e., not mid-computation) when the other half needs it. The paper's specific application interleaves vented additions and carry-xor phases across the two halves to satisfy this constraint.

## When to reach for it

- Any circuit that needs $O(n)$ dirty ancillae but has no external workspace — only the target register itself.
- Classical-quantum addition in qubit-constrained settings (e.g., inside [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] where every qubit is spoken for).
- More generally, any situation where a subroutine needs dirty scratch space and the only candidate is another part of the same register being operated on.

## Complexity

- Doubles the number of sequential phases (process bottom, process top, fix bottom) compared to a single-pass algorithm with external workspace.
- Increases Toffoli count from $3n \pm O(1)$ (with external dirty workspace) to $4n \pm O(1)$ (self-supplied workspace). The overhead is one extra carry-xor pass.
- Adds 1 clean ancilla (to hold the inter-half carry qubit), bringing the total from 2 to 3.

## Caveat

The sequential processing means the two halves cannot be parallelised. This locks you into $O(n)$ depth. If low depth matters more than low ancilla count, use external workspace (or a different adder architecture entirely).

The trick requires careful scheduling of which half is "available" at each phase. In the adder application, Gidney works this out explicitly — but applying the pattern to other circuits requires verifying that the borrowing schedule doesn't create circular dependencies.

## Related notes
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — source paper
- [[Carry Venting via X-Basis Measurement]] — companion trick; venting creates the phasing tasks that require dirty workspace to resolve
- [[Dirty Ancilla Constant Addition]] — the prior CQ adder technique that uses divide-and-conquer with dirty ancillae; this trick is a different way to supply the dirty workspace
- [[Dirty Qubit Recycling for T-Gate Reduction]] — the general principle of borrowing qubits in unknown states
- [[Ancilla Opportunity Cost Analysis]] — relevant when comparing the 3-clean-ancilla variant (this trick) vs. the $n - 2$ dirty ancilla variant
