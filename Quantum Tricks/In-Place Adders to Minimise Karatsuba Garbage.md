# In-Place Adders to Minimise Karatsuba Garbage

> **Source:** Parent, Roetteler, Mosca, arXiv:1706.03419 (2017)
> **Tags:** #trick #quantum-arithmetic #reversible-computation #multiplication #Karatsuba

## What it does

Reduces garbage accumulation at each level of a Karatsuba multiplication recursion by using in-place adders (which overwrite one input with the sum) instead of out-of-place adders (which write the sum to a fresh register).

## The trick

The Karatsuba decomposition $xy = 2^n A + 2^{\lceil n/2 \rceil} B + C$ requires computing $x_0 + x_1$, $y_0 + y_1$, and several subtractions to assemble $B$. If each addition allocates a new register, the garbage per recursion level grows with the number of additions.

Using the [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro in-place adder]] ($|a\rangle|b\rangle \to |a\rangle|a+b\rangle$) instead, the additions overwrite one of the existing registers. The circuit in this paper uses 4 in-place adders of size $n$ and 4 of size $n/2$ at each level, contributing $O(n)$ Toffoli gates per level but **zero additional ancilla qubits** for the addition results.

The only new qubits at each level are those needed for the three half-size multiplication outputs. The recursion for the Toffoli cost becomes:

$$K_n^g = 3 K_{n/2}^g + 4(A_n + A_{n/2})$$

where $A_m = 2m$ is the Toffoli cost of an in-place adder. The final two adders at each level also restore the inputs ($x_0 + x_1$ and $y_0 + y_1$) back to their original values, which is needed to avoid corrupting the inputs for the uncomputation pass.

## When to reach for it

- Any recursive quantum circuit where intermediate combination steps involve additions or subtractions. Using in-place variants prevents garbage from growing multiplicatively across recursion levels.
- Particularly useful when combined with [[Pebble Game Space Reduction for Recursive Circuits|pebble game strategies]], since less garbage per level means the pebble game has less space to manage.
- Applies to reversible implementations of Strassen-like algorithms, polynomial multiplication, and any divide-and-conquer scheme over a ring.

## Complexity

Each in-place addition of $m$-bit numbers costs $2m$ Toffoli gates (using the [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro adder]]) and 1 ancilla qubit. The controlled version costs $4m$ Toffoli gates. No additional output qubits are needed beyond the $2m$-bit product register.

## Caveat

In-place addition destroys one input. If both inputs are needed later (as they are in Karatsuba, since $x_0$ and $x_1$ feed into separate sub-multiplications), the circuit must either (a) compute the sum before the sub-multiplications that would consume $x_0, x_1$, or (b) add a restoration step afterward. The Parent-Roetteler-Mosca circuit handles this by scheduling the final two adders to restore inputs, at the cost of those extra $O(n)$ gates.

## Related notes
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — source paper
- [[MAJ-UMA Decomposition for Reversible Adders]] — the in-place adder used
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — original adder paper
- [[Pebble Game Space Reduction for Recursive Circuits]] — the complementary space optimisation
- [[Reversible Computation via Compute-Copy-Uncompute]] — the cleanup pattern that motivates minimising garbage
