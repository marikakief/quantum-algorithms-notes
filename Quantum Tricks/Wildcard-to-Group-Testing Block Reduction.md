# Wildcard-to-Group-Testing Block Reduction

> **Source:** Ambainis and Montanaro, arXiv:1210.1148
> **Tags:** #trick #reduction #group-testing #wildcard-search #lower-bound

## What it does

Transfers lower bounds from wildcard search to combinatorial group testing by encoding each wildcard bit into a two-item block with exactly one defective.

## The trick

Take a $k$-bit wildcard-search instance $z\in\{0,1\}^k$. Build a group-testing instance with $k$ blocks

$$
B_i=\{2i-1,2i\}.
$$

Promise exactly one defective item in each block. Its position encodes the bit $z_i$.

A group-testing query chooses a subset $S$ of items. For each block, the intersection $S\cap B_i$ has useful choices:

- choose $\{2i-1\}$ to test one value of $z_i$;
- choose $\{2i\}$ to test the other value;
- choose $\varnothing$ to wildcard that coordinate.

The OR answer says whether any tested coordinate matched the chosen value. Negating the response gives the subset-equality wildcard query on the complemented string.

Thus a group-testing algorithm for these promised instances would solve wildcard search.

## When to reach for it

- Lower-bounding sparse-identification or group-testing models.
- Moving from equality-style oracle questions to OR-style subset queries.
- Encoding a Boolean variable as a one-hot pair so that subset OR queries simulate coordinate tests.

## Complexity

A single group-testing query simulates one wildcard query under the block promise. The reduction maps wildcard search on $k$ bits to group testing with $k$ defectives among $2k$ relevant items, so the $\Omega(\sqrt k)$ wildcard lower bound implies an $\Omega(\sqrt k)$ group-testing lower bound.

## Caveat

This is a lower-bound reduction. It does not give a good group-testing algorithm, and it only covers a promised subclass of group-testing inputs.

## Related notes

- [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]]
- [[Neighbour-Edge Adversary for Wildcard Queries]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
