# Use Small Commutator Subgroup to Abelianize HSP

## Pattern

If the commutator subgroup $G'$ is small enough to enumerate, reduce an HSP over $G$ to a normal HSP for $HG'$ plus a lifting step.

Define

$$
F(x)=\{f(xg):g\in G'\}.
$$

This hides $HG'$. Since $G/G'$ is abelian, $HG'$ is normal in $G$. A normal-HSP routine finds $HG'$, and enumeration of $G'$ lets one lift generators of $HG'/G'$ to actual elements of $H$.

## Why it helps

The quotient by $G'$ removes all commutators. If $G'$ is small, the information lost in the quotient can be restored by direct enumeration.

## Where it appears

- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]] — Theorem 11; also yields extraspecial $p$-groups in time polynomial in input size plus $p$.

## Reuse checklist

- Can $G'$ be generated and enumerated within the target runtime?
- Is the group encoded uniquely enough for the theorem's assumptions?
- Can the normal HSP for $HG'$ be solved?
- Can each quotient generator coset be searched for an element in $H$?
