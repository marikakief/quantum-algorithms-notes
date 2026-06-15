# Two's-Complement Carry Erasure for In-Place Adders

> **Source:** Draper, Kutin, Rains, Svore, arXiv:quant-ph/0406142
> **Tags:** #trick #quantum-arithmetic #reversible-computation #uncomputation #carry-erasure

## What it does

Erases the carry ancillae produced during an in-place addition, without needing to store the original input $b$, by exploiting the two's-complement identity $\overline{a + b} + a$ produces the same carry string as $a + b$.

## The trick

After computing $s = a + b$ and writing it over $b$, the carry string $c$ sits in ancillae that must be erased. The identity:

$$a + \overline{s} \equiv a + \overline{a + b} \equiv a - a - b - 1 \equiv \overline{b} \pmod{2^n}$$

means the carry string of the addition $a + \overline{s}$ is exactly $c$ (since $a \oplus \overline{s} \oplus c = \overline{b}$ matches $a \oplus b \oplus c$ up to complementation). So:

Bit-level check: for the original addition, write

$$s_i = a_i \oplus b_i \oplus c_i,\qquad c_{i+1}=\operatorname{maj}(a_i,b_i,c_i).$$

In the reverse-erasure addition with inputs $a_i$ and $\overline{s_i}$ and the same carry-in $c_i$,

$$a_i \oplus \overline{s_i} \oplus c_i = \overline{b_i},$$

and the full-adder equation refines this to

$$a_i+\overline{s_i}+c_i=\overline{b_i}+2c_{i+1}.$$

Thus the next carry is also the original $c_{i+1}$, so the entire carry string is reproduced and can be uncomputed.

1. Complement $s$ (in-place NOT gates on the sum register)
2. Run the carry computation in reverse, using $a$ and $\overline{s}$ as inputs
3. The carry ancillae reset to zero
4. Complement back and clean up

This doubles the depth and gate count of the carry computation, but requires no additional ancillae beyond what the forward pass used.

## When to reach for it

- Any in-place quantum adder that computes carry bits into ancillae and needs to erase them
- Works for any carry-computation subroutine (ripple-carry, carry-lookahead, parallel prefix) — the trick depends only on the two's-complement identity, not the specific adder topology
- Particularly useful when the adder is a subroutine inside a larger reversible computation, where the carries *must* be erased for interference to work

## Complexity

No additional ancillae. Doubles the carry-computation cost (depth and gate count). For the [[Carry-Lookahead Tree for Logarithmic-Depth Addition|QCLA adder]], this takes depth from $\sim 2\log n$ (out-of-place) to $\sim 4\log n$ (in-place). Adds $2n - 2$ NOT gates for the two complementation layers.

## Caveat

Only works for two's-complement addition in $\mathbb{Z}$ or $\bmod 2^n$. For one's-complement addition ($\bmod 2^n - 1$), the carry structure is different because carries wrap around. The [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes|QCLA paper]] handles this case separately (Section 5.5) using a more involved construction.

## Related notes

- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]]
- [[Carry-Lookahead Tree for Logarithmic-Depth Addition]]
- [[Phase Overlapping in Tree Circuits]]
- [[MAJ-UMA Decomposition for Reversible Adders]] — the [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro adder]] uses paired MAJ/UMA gates instead; the UMA phase is the ripple-carry analogue of this erasure trick
- [[Carry Venting via X-Basis Measurement]] — [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes|Gidney (2025)]]'s alternative: erase carries via measurement rather than re-computation
- [[Measurement-Asymmetric Uncomputation]] — the principle behind measurement-based erasure (contrasts with the re-computation approach here)
