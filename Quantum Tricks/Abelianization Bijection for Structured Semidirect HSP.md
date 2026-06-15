# Abelianization Bijection for Structured Semidirect HSP

## Pattern

For a semidirect product $G=A\rtimes B$, find a bijection $\pi:G\to G'$ to an abelian group $G'$ such that:

1. $\pi(H)$ is a subgroup of $G'$ for every hidden subgroup $H$ in the target family;
2. the oracle $f\circ\pi^{-1}$ is constant and distinct on cosets of $\pi(H)$;
3. $\pi^{-1}$ is implementable coherently from the given generators.

Then the HSP over $G$ reduces to the abelian HSP over $G'$.

## Why it helps

The map need not be a group homomorphism on all of $G$. It only needs to send the hidden-subgroup coset partition to a valid abelian hidden-subgroup coset partition. This is weaker and can hold for structured semidirect products.

## Where it appears

- [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes]] — reduces $\mathbb Z_{p^r}^m\rtimes\mathbb Z_p$ to $\mathbb Z_{p^r}^m\times\mathbb Z_p$ under a separated-generator input promise.

## Reuse checklist

- Prove $\pi(H)$ is a subgroup, not just a set.
- Prove identical and distinct cosets are respected by $\pi$.
- Check the input model supplies enough generator information to implement $\pi^{-1}$.
- After solving in $G'$, map generators back and verify them against the original oracle.
