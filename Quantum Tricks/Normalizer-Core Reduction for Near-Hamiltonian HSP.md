# Normalizer-Core Reduction for Near-Hamiltonian HSP

## Pattern

For a finite group $G$, define

$$
M_G=\bigcap_{K\leq G}N(K),
$$

where $N(K)=\{g\in G:gKg^{-1}=K\}$ is the normalizer. Since $M_G$ lies in the normalizer of every subgroup, any hidden subgroup $H\leq G$ satisfies

$$
H\triangleleft HM_G.
$$

Thus a nonnormal HSP over $G$ can be split into two tasks:

1. find the quotient image $HM_G/M_G\leq G/M_G$;
2. run a normal-HSP routine inside $HM_G$ to recover $H$.

## Why it helps

The hard part of a nonabelian HSP is often that $H$ is not normal. The subgroup $M_G$ is a group-wide normalizer core: adjoining it to $H$ makes $H$ normal in the larger subgroup $HM_G$. If $G/M_G$ or $G/HM_G$ is small enough to search or sample, finding $HM_G/M_G$ is feasible.

## Where it appears

- [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes]] — main reduction from near-Hamiltonian HSP to normal HSP.
- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]] — supplies related normal-HSP and black-box group tools.

## Reuse checklist

- Can you compute or represent $M_G$?
- Can you enumerate or sample cosets of $G/M_G$?
- Can you test subgroup membership in generated subgroups?
- Is a normal-HSP algorithm available for the generated subgroups $HM_G$ or $U_{G/M_G}(T)$?
