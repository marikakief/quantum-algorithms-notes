# Controlled-Swap Tree for Variable-Dependent Diffusion

> **Source:** Campbell, Khurana, Montanaro, arXiv:1810.05582
> **Tags:** #trick #quantum-walk #diffusion #controlled-swap #backtracking

## What it does

Implements a diffusion operator $D_c$ that depends on a quantum register $|c\rangle$ (e.g., the number of colours used so far in a graph colouring), without expensive conditional branching.

## The trick

The problem: at each step of the [[Quantum Walk on Backtracking Trees via Effective Resistance|backtracking quantum walk]], the diffusion reflects about a state $|\psi_c\rangle$ that depends on the parameter $c$ stored in a quantum register. You can't just "if $c = 3$, do $D_3$" because $c$ is in superposition.

Solution:

1. **Precompute** eigenvectors $|e_i\rangle$ of each $D_i$ (for $i = 0, \ldots, k-1$) with eigenvalue 1. These are prepared once and reused across all walk steps.

2. **Controlled-swap tree:** Store all $|e_i\rangle$ in separate ancilla registers. Using the binary representation of $c$ (an $r$-bit register), perform a depth-$r$ tree of controlled swaps:
   - At level $j$: controlled on the $j$-th bit of $c$, swap the working register $|a'\rangle$ into the register labelled by the first $j$ bits of $c$
   - After $r$ levels, $|a'\rangle$ has been swapped into position $|e_c\rangle$

3. **Apply all $D_i$ in parallel** to each register (cheap — they're independent).

4. **Reverse the swap tree** to put $|a'\rangle$ back, now with $D_c$ applied.

The swap tree has depth $r = \lceil\log(k+1)\rceil$ and uses controlled-swap gates (each decomposing into 3 Toffolis). Only swaps where $c_j = 1$ are performed, because registers with $c_j = 0$ can be associated with their $(j+1)$-bit extension ending in 0 for free.

## When to reach for it

- Any quantum walk where the diffusion operator depends on a quantum parameter
- Graph colouring: diffuse over $c + 1$ colours where $c$ is the number of colours already used
- More broadly: any circuit where you need to apply one of $k$ unitaries selected by a quantum register, and each unitary has a known fixed point

The trick also appears implicitly in other contexts (e.g., QROM lookups), but the eigenvector preparation makes it specific to reflections/diffusions.

## Complexity

Depth: $O(\log k)$ levels of controlled swaps. Each level: 3 Toffolis per swap, all swaps at the same level in parallel. Total Toffoli depth: $O(\log k)$. Space: $k - 1$ ancilla registers of $r + 2$ qubits each.

## Caveat

Requires precomputing and storing the eigenvectors $|e_i\rangle$, which use $O(kr)$ qubits. For large $k$ this isn't negligible, but for the graph colouring application ($k \leq 24$ for $n \leq 200$) it's manageable.

## Related notes
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]]
- [[Quantum Walk on Backtracking Trees via Effective Resistance]]
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]
