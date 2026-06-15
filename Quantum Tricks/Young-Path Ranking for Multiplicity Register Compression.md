# Young-Path Ranking for Multiplicity Register Compression

> **Source:** Bacon--Chuang--Harrow, arXiv:quant-ph/0601001
> **Tags:** #trick #representation-theory #compression #Young-tableaux #Schur-transform

## What it does
Compresses a Young--Yamanouchi path for $P_\lambda$ into an integer in $[\dim P_\lambda]$ by ranking it among all valid paths.

## The trick
A Young--Yamanouchi basis vector for $P_\lambda$ is a path

$$
p=(p_n=\lambda,p_{n-1},\ldots,p_1=(1)),
$$

where $p_{k-1}\in p_k-\square$. Fix a total order on partitions of each size, for example lexicographic order. Rank paths by comparing $p_{n-1}$ first, then $p_{n-2}$, and so on.

BCH give the ranking function

$$
f_n(p_1,\ldots,p_n)=1+
\sum_{k=2}^{n}
\sum_{\substack{\mu\in p_k-\square\\ \mu<p_{k-1}}}
\dim P_\mu.
$$

The dimensions $\dim P_\mu$ are efficiently computable from the Young diagram, so $f_n$ can be implemented reversibly in polynomial time. This maps the path register to a compact integer register of size $\lceil\log \dim P_\lambda\rceil$.

## When to reach for it
- Schur-transform applications where the multiplicity register must be close to information-theoretically minimal.
- Universal compression or entanglement-concentration circuits where leaving the raw path register would waste space.
- Any multiplicity-free branching graph where path ranking can be computed from subtree sizes.

## Complexity
The formula has $O(n^2)$ candidate terms, and each dimension term can be computed in polynomial time from the Young diagram. BCH treat the whole compression as an optional $\operatorname{poly}(n)$ overhead.

## Caveat
Compression is not needed for every Schur-transform use. If asymptotic workspace is not the limiting cost, the row-addition history $|j_1,\ldots,j_{n-1}\rangle$ is simpler and avoids extra reversible arithmetic.

## Related notes
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]
- [[Subgroup-Adapted Basis Encoding for Schur-Weyl Registers]]
- [[Clebsch-Gordan Cascade for Schur Transforms]]
