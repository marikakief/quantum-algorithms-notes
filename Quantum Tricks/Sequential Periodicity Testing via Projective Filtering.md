# Sequential Periodicity Testing via Projective Filtering

> **Source:** Ettinger, Høyer, Knill, arXiv:quant-ph/0401083 (2004)
> **Tags:** #trick #hidden-subgroup-problem #projective-measurement #query-complexity

## What it does

Identifies which of $r$ candidate subgroups is the hidden subgroup of a function $f$, using $O(\log r)$ copies of the coset state, by sequentially projecting onto coset states of each candidate.

## The trick

Given $s$ copies of the coset state $\frac{1}{\sqrt{|H|}}\sum_{h \in H} |th\rangle|f(t)\rangle$ and an ordered list of candidate subgroups $K_1, \ldots, K_r$ (largest first):

1. For each candidate $K_\mu$ in order, project the $s$ subgroup registers onto the coset-state subspace of $K_\mu$:
$$P_{s,\mu} = \left(\sum_{t \in T_\mu} |tK_\mu\rangle\langle tK_\mu| \otimes I\right)^{\otimes s}$$
2. If the projection succeeds (the state lies in the $+1$-eigenspace), record $K_\mu$ as the answer
3. If it fails, the state is perturbed by at most $\|{\Delta}\| \leq 2/2^{s/2}$ — small enough to continue testing

**Why the ordering matters:** Testing larger subgroups first prevents false positives. If $K_\mu \supsetneq H$, then the coset states of $H$ partially overlap with those of $K_\mu$, but $|K_\mu \cap H|/|K_\mu| \leq 1/2$ (because $H$ is a proper subgroup). Over $s$ copies, the false-positive probability is $(1/2)^s$.

**Why it works:** The total accumulated perturbation after $r$ failed tests is at most $2r/2^{s/2}$. With $s = O(\log r)$ copies, this is exponentially small. For $r \leq 2^{O(n^2)}$ subgroups of a group of order $2^n$, we need $s = O(n^2)$ copies.

## When to reach for it

- When you have multiple copies of a quantum state and need to identify which of several candidates it belongs to
- The [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|standard HSP approach]] (Fourier transform + measure) doesn't work for non-abelian groups, but this technique handles all groups information-theoretically
- Situations where you can afford exponential post-processing time but want polynomial query count

## Complexity

$O(\log r)$ copies (queries) where $r$ is the number of candidates. Post-processing is exponential in general (constructing the projectors requires knowledge of all subgroups).

## Caveat

The projectors $P_{s,\mu}$ are generally exponential-dimensional and can't be implemented efficiently. This technique proves information-theoretic results about query complexity but doesn't give efficient algorithms. For efficient HSP algorithms, you need structure-specific approaches like the [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|QFT]] (abelian groups) or [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|quantum sieves]] (dihedral groups).

## Related notes

- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Error-to-Exact Conversion via Conditional Rotation]]
