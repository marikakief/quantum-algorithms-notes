# Normal-Subgroup Fallback for Solvable HSP

> **Source:** Cosme--Portugal, arXiv:quant-ph/0703223; uses the normal-subgroup algorithmic route for solvable groups
> **Tags:** #trick #hidden-subgroup-problem #solvable-groups #normal-subgroups

## What it does

Avoids a custom nonabelian measurement when the remaining hidden-subgroup candidates are known to be normal in a solvable group.

## The trick

After abelian restrictions and subgroup classification, a nonabelian HSP can leave a small unresolved family. If every subgroup in that family is normal in the ambient solvable group $G$, stop designing case-specific slope tests and call the normal-HSP machinery for solvable groups.

In Cosme--Portugal, the unresolved class-(1) subgroups of $\mathbb{Z}_{p^r}\rtimes\mathbb{Z}_{p^2}$ all contain the commutator subgroup

$$
G'=\langle x^{p^{r-2}}\rangle,
$$

hence are normal. Since $G$ is a finite $p$-group, it is solvable. The normal hidden subgroup can then be found in time polynomial in $\log |G|$ by reduction to abelian HSP instances.

## When to reach for it

Use it in classified solvable groups where:

1. abelian restrictions already reveal enough parameters to identify a short candidate list;
2. the remaining candidates are normal; and
3. a normal-HSP algorithm for the group class is available.

## Complexity

The fallback preserves polynomial dependence on $\log |G|$ when the solvable-group normal-HSP routine is efficient. It also reduces proof burden: one does not need a separate Fourier-sampling derivation for each leftover subgroup shape.

## Caveat

Normality is the whole test. If the candidate list contains non-normal subgroups, this shortcut can lose the hidden conjugate parameter.

## Related notes

- [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
