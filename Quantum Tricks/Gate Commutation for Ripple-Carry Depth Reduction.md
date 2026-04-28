# Gate Commutation for Ripple-Carry Depth Reduction

> **Source:** Cuccaro, Draper, Kutin, Moulton, arXiv:quant-ph/0410184
> **Tags:** #trick #circuit-optimisation #depth-reduction #ripple-carry #quantum-arithmetic

## What it does

Halves the effective depth of a ripple-carry chain by exploiting commutation relations between adjacent gate blocks, allowing Toffoli gates in one block to execute in parallel with CNOT gates two blocks ahead.

## The trick

In a ripple-carry adder, the upward sweep is a chain of identical MAJ blocks. Within each MAJ block, the final Toffoli gate (which writes the carry) acts on different qubits than the initial CNOT of the *next* MAJ block — but the circuit naively serialises them because they sit in adjacent blocks.

The observation: the Toffoli at the end of MAJ$_i$ commutes with the second CNOT of MAJ$_{i+1}$, because they act on disjoint qubit sets (the Toffoli targets $A_i$ using controls $A_{i-1}$ and $B_i$; the CNOT in the next block targets $B_{i+1}$ from $A_{i+1}$).

Swap their order. Now the Toffoli of block $i$ can run in the same time-slice as the CNOT of block $i + 2$. The same trick applies symmetrically to the UMA (downward) sweep.

Combined with parallelising all initial CNOTs into one time-slice and all final CNOTs into another, this reduces the adder depth from $\sim 4n$ (naive) to $2n + 4$.

## When to reach for it

- Any quantum circuit built from repeated blocks where adjacent blocks share no qubit operands in their boundary gates. This is common in ripple-carry arithmetic (adders, subtractors, comparators) and shift-and-add multipliers.
- When you need to reduce depth without adding ancillae — this is a free optimisation that costs nothing.
- As a first pass before applying more aggressive optimisations (like [[Temporary Logical-AND]] for T-count or carry-lookahead for $O(\log n)$ depth).

## Complexity

Saves $\sim n$ time-slices from the naive $\sim 4n$ depth, yielding depth $2n + O(1)$. No extra qubits or gates — purely a scheduling improvement.

## Caveat

Only works when the boundary gates of adjacent blocks genuinely commute. If the blocks share qubit operands at their boundaries (as in some non-standard carry structures), the swap is invalid. Always verify the commutation relation for the specific circuit.

The technique does not break the $\Omega(n)$ depth barrier inherent to ripple-carry. For $O(\log n)$ depth, you need a fundamentally different adder topology.

## Related notes
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — source paper
- [[In-Place Majority for Carry Propagation]] — the MAJ gate whose Toffoli is being commuted
- [[MAJ-UMA Decomposition for Reversible Adders]] — the overall adder structure this optimises
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — builds on the Cuccaro adder with further T-count optimisations
