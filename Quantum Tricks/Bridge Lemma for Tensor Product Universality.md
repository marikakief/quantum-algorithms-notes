# Bridge Lemma for Tensor Product Universality

> **Source:** Aharonov & Arad, arXiv:quant-ph/0605181
> **Tags:** #trick #universality #density #quantum-gates

## What it does

Provides a simple criterion for when local gate sets generate a dense subgroup of $SU(V \otimes W)$: you need irreducibility on each factor separately, plus one "bridge" element that doesn't respect the tensor product structure.

## The trick

**Lemma:** Let $G \leq GL(V \otimes W)$ be a group. If:
1. $G$ acts irreducibly on $V$ (i.e., the restriction to $V \otimes |w_0\rangle$ for any fixed $|w_0\rangle$ generates all of $\text{End}(V)$),
2. $G$ acts irreducibly on $W$, and
3. There exists $g \in G$ that does not preserve the tensor product structure (i.e., $g \neq A \otimes B$ for any $A, B$),

then $G$ acts irreducibly on $V \otimes W$.

With additional arguments (e.g., the group being compact and connected), irreducibility implies density in $SU(V \otimes W)$.

## When to reach for it

- Proving quantum universality from a small gate set: show single-qubit universality on each subsystem, then find one entangling gate.
- Any density-in-$SU(n)$ argument where the system has a tensor product structure.
- Generalising universality proofs to larger systems from local universality.

## Complexity

No computational cost — this is a proof technique.

## Caveat

The "bridge" condition (3) can be subtle to verify. For gates close to the identity (as in the large-$k$ path model), you may need the [[Decoupling via Iterated Commutators]] technique to extract a non-trivial bridge element from products and commutators of the generators.

## Related notes
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes]] — original unitary version
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]] — reproves the Bridge Lemma for $SL(n, \mathbb{C})$ and $SL(n, \mathbb{R})$ (non-unitary case); constructive proof via four subsidiary lemmas (Appendix A.1)
- [[Decoupling via Iterated Commutators]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Jørgensen Inequality for Non-Unitary Universality Seeding]] — the 2D seed that the Bridge Lemma builds upon
