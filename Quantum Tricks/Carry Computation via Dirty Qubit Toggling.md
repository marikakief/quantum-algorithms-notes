# Carry Computation via Dirty Qubit Toggling

> **Source:** Häner, Roetteler, Svore, arXiv:1611.07995
> **Tags:** #trick #quantum-arithmetic #dirty-qubits #Toffoli #reversible-computation

## What it does

Computes the carry (overflow) bit of $a + c$ for a classical constant $c$ and quantum register $|a\rangle$, using $n - 1$ borrowed dirty qubits that are returned to their original (unknown) state.

## The trick

Since dirty qubits are in an unknown state, you cannot *set* them — but you can *toggle* them and later un-toggle them. The carry is encoded as a chain of toggles.

For each bit position $i$, a carry from bit $i$ to bit $i+1$ occurs when at least two of $\{a_i, c_i, g_{i-1}\}$ are 1, where $g_{i-1}$ represents the carry from the previous position (encoded as a toggle of dirty qubit $g_{i-1}$).

The circuit for each bit $i$:
- **If $c_i = 1$:** Apply NOT to $a_i$, then CNOT (control $a_i$, target $g_i$), then Toffoli (controls $a_i$ and $g_{i-1}$, target $g_i$). The NOT+CNOT handles the case $a_i = c_i = 1$; the Toffoli handles carry propagation from $g_{i-1}$.
- **If $c_i = 0$:** Apply only the Toffoli (controls $a_i$ and $g_{i-1}$, target $g_i$). The only way to generate a carry is $a_i = g_{i-1} = 1$.

The Toffoli that depends on $g_{i-1}$'s toggle state is placed *before* and *after* the potential toggle of $g_{i-1}$: if $g_{i-1}$ actually toggled, both Toffolis fire and cancel; if $g_{i-1}$ did not toggle, neither fires. This "bracket" pattern is what makes toggling equivalent to reading the carry value, despite the qubit being in an unknown state.

After the forward sweep computes the final carry in $a_{n-1}$, the entire circuit runs in reverse (minus the output gate) to restore all $a_i$ and $g_i$ qubits.

**Optimisation:** The first dirty qubit $g_0$ can be eliminated by conditioning the first Toffoli directly on $a_0$ instead of testing for toggling of $g_0$.

**Toffoli count:** $4(n - 2) + 2$ (including uncomputation of dirty qubits).

## When to reach for it

- As the inner primitive for [[Dirty Ancilla Constant Addition]], where the carry gate feeds a conditional incrementer at each recursion level.
- Whenever you need to compare a quantum value against a classical constant: the comparator $a < c$ is equivalent to computing the carry of $a + (2^n - c)$.
- In any setting where the only available scratch qubits are dirty (entangled with other parts of the computation).

## Complexity

- **Toffoli count:** $4(n - 2) + 2 = 4n - 6$
- **Depth:** $O(n)$
- **Ancillae:** $n - 1$ dirty qubits (all returned to original state)

## Caveat

The circuit is specifically for adding/comparing against a *classical* constant $c$. The gate structure (which gates are present, which are absent) depends on the binary representation of $c$, determined at circuit-generation time. For quantum-quantum comparisons, use a different approach (e.g., the [[MAJ-UMA Decomposition for Reversible Adders|Cuccaro adder]] applied to subtraction).

The $4n - 6$ Toffoli count (with uncomputation) is higher than the $2n - 1$ Toffolis of Cuccaro's adder, but Cuccaro requires clean ancillae whereas this uses only dirty ones.

## Related notes
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — source paper
- [[Dirty Ancilla Constant Addition]] — uses this as the base primitive in a divide-and-conquer adder
- [[In-Place Majority for Carry Propagation]] — the alternative (clean-ancilla) carry propagation from Cuccaro
- [[MAJ-UMA Decomposition for Reversible Adders]] — the paired-gate carry structure for quantum-quantum addition
- [[Dirty Qubit Recycling for T-Gate Reduction]] — the general principle of borrowing idle qubits as dirty ancillae
