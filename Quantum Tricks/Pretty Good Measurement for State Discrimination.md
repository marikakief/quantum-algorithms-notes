# Pretty Good Measurement for State Discrimination

> **Source:** Bacon, Childs & van Dam, arXiv:quant-ph/0504083; Hausladen & Wootters (1994)
> **Tags:** #trick #measurement #state-discrimination #HSP

## What it does

Provides a near-optimal (often exactly optimal) POVM for distinguishing members of a quantum state ensemble, with a simple closed-form construction.

## The trick

Given an ensemble of states $\{\sigma_j\}$ with equal prior probabilities, the pretty good measurement (PGM) is the POVM:

$$
E_j = \Sigma^{-1/2} \sigma_j \Sigma^{-1/2}, \quad \Sigma = \sum_j \sigma_j
$$

where the inverse is taken on the support of $\Sigma$.

The PGM is "pretty good" in a precise sense: its success probability is at least the square of the optimal measurement's success probability (Barnum & Knill 2002). For many natural ensembles — including hidden subgroup states over semidirect product groups — it's exactly optimal.

Implementation: diagonalise $\Sigma$, compute $\Sigma^{-1/2}$, apply it to each $\sigma_j$, and measure in the resulting basis. For the HSP, diagonalising $\Sigma$ reduces to solving an algebraic problem (the matrix sum problem), connecting measurement design to classical algebra.

## When to reach for it

- Quantum state discrimination with equal priors.
- Hidden subgroup problem: the PGM gives the optimal strategy for extracting information from hidden subgroup states.
- Any setting where you need to distinguish quantum states and want a good, easily characterised measurement without optimising over all POVMs.

## Complexity

Implementing the PGM requires diagonalising $\Sigma$, which has cost depending on the ensemble structure. For HSP over $A \rtimes \mathbb{Z}_p$, this reduces to the matrix sum problem.

## Caveat

"Pretty good" doesn't mean "optimal" in general — there exist ensembles where the PGM is strictly suboptimal. But for the important HSP cases (dihedral, semidirect products with cyclic factor), it's provably optimal.

## Related notes
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
- [[Entangled Multi-Copy Measurements for Nonabelian HSP]]
