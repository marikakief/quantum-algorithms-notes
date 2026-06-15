# Basis-Selected Strong Fourier Sampling for Affine HSP

> **Source:** Moore--Rockmore--Russell--Schulman, arXiv:quant-ph/0211124
> **Tags:** #trick #hidden-subgroup-problem #fourier-sampling #nonabelian #affine-groups

## What it does

Extracts a hidden translation parameter in affine groups by measuring row data in a basis chosen for the high-dimensional representation.

## The trick

In the affine group

$$
A_p=\mathbb{Z}_p^*\ltimes\mathbb{Z}_p,
$$

conjugates of the largest non-normal subgroup have the form

$$
H_b=\{(a,(1-a)b):a\in\mathbb{Z}_p^*\}.
$$

Weak Fourier sampling cannot distinguish the different $b$ values. The high-dimensional irrep $\rho$ has projection columns that are phase multiples of

$$
(u_b)_j=\frac{1}{p-1}\omega_p^{bj}.
$$

So the measurement should not stop at the irrep name:

1. Perform the nonabelian Fourier transform over $A_p$.
2. Condition on the $(p-1)$-dimensional irrep.
3. Measure the column to remove the random coset action.
4. Fourier transform the row index over $\mathbb{Z}_{p-1}$.
5. Use the measured frequency $\ell$ to infer $b$ from the approximation

$$
\frac{b}{p}\approx\frac{\ell}{p-1}.
$$

## When to reach for it

Use it when the hidden subgroup family consists of conjugates and weak sampling sees only their shared normal data. The row/column basis can carry the conjugating parameter.

## Complexity

For the largest affine stabilizer, one good frequency occurs with probability at least $(2/\pi)^2$ after conditioning on the high-dimensional irrep, and that irrep appears with probability $1-1/p$.

## Caveat

The basis is part of the algorithm. A random basis or an abelian direct-product transform can erase the translation information.

## Related notes

- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]
- [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
