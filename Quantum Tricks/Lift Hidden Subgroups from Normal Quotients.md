# Lift Hidden Subgroups from Normal Quotients

## Pattern

Let $N\triangleleft G$ be a known or recoverable normal subgroup. To find a hidden subgroup $H\leq G$, separately recover:

$$
H\cap N
$$

and

$$
HN/N\leq G/N.
$$

Then lift enough generators of $HN/N$ back into actual elements of $H$. If the lifted subgroup $H_1\leq H$ satisfies

$$
H_1\cap N=H\cap N,
\qquad
H_1N=HN,
$$

then $H_1=H$.

## Why it helps

The quotient $G/N$ may be abelian, cyclic, small, or otherwise easier than $G$. The intersection $H\cap N$ may also be an abelian HSP. The original nonabelian HSP is solved by matching the intersection and quotient image.

## Where it appears

- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]] — used for small commutator subgroups and elementary abelian normal 2-subgroups.
- [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes]] — uses the special case $N=M_G$ with $H\triangleleft HM_G$.

## Reuse checklist

- Can $H\cap N$ be found efficiently?
- Can $HN/N$ be found efficiently in the quotient?
- Can quotient generators be lifted to elements that really lie in $H$?
- Is there a proof that matching intersection and image forces equality?
