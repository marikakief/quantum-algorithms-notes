# MAJ-UMA Decomposition for Reversible Adders

> **Source:** Cuccaro, Draper, Kutin, Moulton, arXiv:quant-ph/0410184
> **Tags:** #trick #quantum-arithmetic #ripple-carry #reversible-computation #circuit-primitive

## What it does

Decomposes a reversible ripple-carry adder into matched pairs of MAJ (majority) and UMA (unmajority-and-add) gates, enabling a single-ancilla adder with $2n + O(1)$ Toffoli gates instead of the $4n + O(1)$ Toffolis of VBE.

## The trick

A ripple-carry adder has two phases: an upward sweep computing carries $c_1, \ldots, c_n$, and a downward sweep erasing carries while writing sum bits. In VBE's design, each carry gets its own ancilla qubit. The MAJ-UMA decomposition avoids this by storing each carry in the input register $A_i$ (overwriting $a_i$), then restoring $a_i$ during the downward sweep.

**MAJ** (2 CNOTs + 1 Toffoli): takes $(c_i, b_i, a_i)$ to $(c_i \oplus a_i, \; b_i \oplus a_i, \; c_{i+1})$. Stores the new carry in $A_i$.

**UMA** (1 Toffoli + 2 or 3 CNOTs): takes $(c_i \oplus a_i, \; b_i \oplus a_i, \; c_{i+1})$ back to $(c_i, \; s_i, \; a_i)$. Restores $a_i$, erases the carry, and writes $s_i = a_i \oplus b_i \oplus c_i$ into $B_i$.

The full adder: apply MAJ for $i = 0, 1, \ldots, n-1$ (upward sweep), copy the final carry $c_n$ to the output, then apply UMA for $i = n-1, \ldots, 0$ (downward sweep). Each MAJ-UMA pair uses 2 Toffolis, giving $2n + O(1)$ total.

The paired structure — each Toffoli in MAJ has a matching Toffoli in UMA — is what later allowed [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] to replace each pair with a [[Temporary Logical-AND]] (4 T gates compute, 0 T gates erase), cutting the T-count from $8n$ to $4n$.

## When to reach for it

- Designing any ripple-carry quantum adder or subtractor where ancilla count matters.
- As a template for other reversible computations with a similar "propagate forward, clean up backward" structure — e.g., comparators, ripple-carry multipliers.
- When analysing whether a circuit's Toffoli gates come in compute/uncompute pairs (if they do, the [[Temporary Logical-AND]] trick applies).

## Complexity

$2n - 1$ Toffoli gates + $5n - 3$ CNOTs + $2n - 4$ NOTs for the optimised $n$-bit adder. Depth $2n + 4$. One ancilla qubit.

## Caveat

The decomposition is specific to ripple-carry (linear-depth) adders. It does not help with carry-lookahead or other logarithmic-depth topologies, which use different carry computation structures. For circuits where depth matters more than ancilla count, carry-lookahead (Draper-Kutin-Rains-Svore) is better despite needing $O(n)$ ancillae.

## Related notes
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — source paper
- [[In-Place Majority for Carry Propagation]] — the MAJ half of the decomposition
- [[Gate Commutation for Ripple-Carry Depth Reduction]] — depth optimisation applied to MAJ/UMA chains
- [[Temporary Logical-AND]] — the T-count optimisation that exploits the paired Toffoli structure
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — applies [[Temporary Logical-AND]] to this decomposition
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — the alternative Fourier-basis approach
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — uses this adder in a Karatsuba multiplier; see [[In-Place Adders to Minimise Karatsuba Garbage]]
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — uses a different carry approach ([[Carry Computation via Dirty Qubit Toggling]]) to avoid clean ancillae entirely
