# Function Isomorphism Testing via Orbit States

> **Source:** Harrow--Lin--Montanaro, arXiv:1607.03236
> **Tags:** #trick #property-testing #function-isomorphism #group-actions #query-complexity

## What it does

Tests whether two functions are isomorphic under a group action by encoding them as quantum states and checking for a unitary orbit relation.

## The trick

For $f,g:X\to Y$, prepare

$$
|f\rangle=\frac1{\sqrt{|X|}}\sum_{x\in X}|x\rangle|f(x)\rangle,
\qquad
|g\rangle=\frac1{\sqrt{|X|}}\sum_{x\in X}|x\rangle|g(x)\rangle.
$$

For each $\sigma\in G$, define the permutation unitary $U_\sigma|x\rangle=|\sigma(x)\rangle$. On

$$
|\psi\rangle=\frac1{\sqrt2}(|0\rangle|f\rangle+|1\rangle|g\rangle),
$$

construct $U'_\sigma$ so that

$$
\langle\psi|U'_\sigma|\psi\rangle=\langle f\circ\sigma|g\rangle=1-d(f\circ\sigma,g).
$$

Then use [[Hadamard Invariance Test for Unitary Orbits]] plus an OR over $\sigma$.

## When to reach for it

- Testing Boolean function isomorphism, linear isomorphism, affine isomorphism.
- Graph isomorphism under an $\epsilon$-far property-testing promise.
- HSP-flavoured property testing where standard coset-state reductions would be too expensive.

## Complexity

The query complexity is

$$
O((\log |G|)/\epsilon),
$$

because each orbit-state copy costs one query to $f$ and one to $g$.

## Caveat

This is not full isomorphism decision. It is a property tester with a promise that no-instances are $\epsilon$-far from every group translate.

## Related notes

- [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]]
