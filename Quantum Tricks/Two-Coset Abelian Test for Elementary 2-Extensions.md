# Two-Coset Abelian Test for Elementary 2-Extensions

## Pattern

Let $N\triangleleft G$ be an elementary abelian normal $2$-subgroup, and suppose $H\leq G$ is hidden. After finding $H\cap N$, test whether a quotient representative $zN$ contains an element of $H$ by defining a function on the abelian group $\mathbb Z_2\times N$:

$$
F(0,x)=f(x),\qquad F(1,x)=f(xz).
$$

If $zN\cap H$ is empty, this hides

$$
\{0\}\times(H\cap N).
$$

If $zN\cap H$ is nonempty, it hides a larger subgroup of the form

$$
(\{0\}\times(H\cap N))\cup(\{1\}\times u(H\cap N)),
$$

where $u\in zH\cap N$. A generator $(1,u)$ gives $u^{-1}z\in H$.

## Why it helps

The test converts a nonabelian lifting problem into an abelian HSP. The exponent-$2$ condition is what makes the union of the two cosets a subgroup of $\mathbb Z_2\times N$.

## Where it appears

- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]] — Theorem 13.

## Reuse checklist

- $N$ is elementary abelian of exponent $2$ and given by generators.
- $H\cap N$ is already known or can be found by abelian HSP.
- You have candidate quotient representatives $z$ that cover the possible subgroup images in $G/N$.
