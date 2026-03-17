
> **Tags:** #trick #sorting #reversible-computing #ancilla
> **Source:** npj Quantum Information 2018 (s41534-018-0071-5)

## What it does
Implements a [[Reversible Comparator with Swap Record|comparator]] reversibly by storing whether a swap was required in one ancilla bit.

## The trick
For two registers $A,B$:
1. Compute comparison bit $c = [A>B]$ into ancilla.
2. Conditionally swap $(A,B)$ if $c=1$.

Outputs sorted pair $(\min(A,B),\max(A,B))$ plus record bit $c$.

This makes comparison reversible and replayable.

## Why it matters
The stored bit is not just garbage: it is a **control transcript** that can later be replayed in reverse (e.g., reverse-sort to generate permutations with sign).

## When to use
- Any reversible [[Sorting Networks as Quantum Control-Flow Compilers|sorting network]]
- Reversible data-dependent transforms where you need exact undo
- Coherent branch logging for later controlled replay

## Cost
[[Reversible Comparator with Swap Record|comparator]] cost scales with register size ($O(\log N)$ gates); depth can be $O(\log\log N)$ with optimized arithmetic/comparison circuits.

## Related Paper Notes

- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]]
