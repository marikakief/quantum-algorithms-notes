# Cosine-Bias Sampling for Dihedral Hidden Reflections

## Pattern

For a hidden reflection $H=\{(0,0),(k_0,1)\}\leq D_N$, a simple Fourier/Hadamard experiment does not output $k_0$ directly. It outputs samples whose distribution is biased by $k_0$:

$$\Pr[(a,0)]={1\over N}\cos^2(\pi k_0a/N),\qquad
\Pr[(a,1)]={1\over N}\sin^2(\pi k_0a/N).$$

The hidden subgroup has been converted into a statistical identification problem over $\mathbb Z_N$.

## How to use it

1. Reduce the dihedral HSP to the hidden-reflection case by first finding the rotation part with abelian HSP.
2. Run the Fourier/Hadamard query experiment many times.
3. Condition on one branch, usually the cosine branch.
4. Test candidate $k$ values by the correlation statistic
   $$\sum_i \cos(2\pi k z_i/N).$$
5. Use concentration bounds to show that the true $k_0$ separates from the wrong candidates with $O(\log N)$ samples.

## Why it matters

This gives query-efficient identification without efficient post-processing. It is the clean example where the quantum part has enough information, but classical extraction is the hard step.

## Source papers

- [[On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998) — Paper Notes|Ettinger--Høyer (1998)]].
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger--Høyer--Knill (2004)]].
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]].
