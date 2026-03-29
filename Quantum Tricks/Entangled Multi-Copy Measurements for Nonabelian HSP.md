# Entangled Multi-Copy Measurements for Nonabelian HSP

> **Source:** Bacon, Childs & van Dam, arXiv:quant-ph/0504083
> **Tags:** #trick #HSP #entangled-measurement #nonabelian

## What it does

Extracts information from hidden subgroup states by performing entangled (joint) measurements across multiple copies, succeeding where single-copy measurements provably fail.

## The trick

For the nonabelian HSP, the hidden subgroup state $\rho_d$ on a single copy may not contain enough accessible information to identify $d$ — specifically, for the symmetric group, no single-copy measurement can solve the HSP efficiently (Moore, Russell, Schulman 2005).

The fix: perform the [[Pretty Good Measurement for State Discrimination|pretty good measurement]] on $\rho_d^{\otimes r}$ (the joint state of $r$ copies). The tensor product state lives in a larger Hilbert space where the subgroup $d$ can be resolved.

For $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$, the PGM on $r$ copies reduces to solving a system of polynomial equations over $\mathbb{F}_p$ (the matrix sum problem), which can be done classically via Gröbner bases. The implementation is:

1. Prepare $r$ copies of the hidden subgroup state.
2. Fourier transform each copy on the abelian register.
3. Solve the matrix sum problem to identify the correct measurement basis.
4. Measure.

The key: $r$ copies are necessary and sufficient. The algebraic structure of the group determines exactly how many copies are needed.

## When to reach for it

- Nonabelian HSP where single-copy measurements don't suffice.
- Any quantum state discrimination problem where joint measurements on multiple copies outperform independent measurements.
- Designing optimal measurement strategies for structured quantum state ensembles.

## Complexity

Requires $r$ copies of $\rho_d$ (polynomial in input size when $r$ is fixed). The classical post-processing (Gröbner basis computation) is polynomial for fixed $r$ but grows with $r$.

## Caveat

For the dihedral group, the PGM is optimal and the number of copies needed is $O(\log N)$, but the resulting matrix sum problem is average-case subset sum — believed to be hard. So optimal measurement + hard classical post-processing = no efficient algorithm. The technique gives efficient algorithms only when the matrix sum problem is tractable.

## Related notes
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
- [[Pretty Good Measurement for State Discrimination]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
