# Random Isolation plus Bernstein-Vazirani for Group Testing

> **Source:** Ambainis and Montanaro, arXiv:1210.1148
> **Tags:** #trick #group-testing #Bernstein-Vazirani #random-restriction #Las-Vegas

## What it does

Removes the $\log n$ dependence from combinatorial group testing by randomly restricting the universe until a subset contains exactly one defective item, then identifying it with one quantum query.

## The trick

If $|x|\le 1$, a group-testing query on $S$ computes

$$
\bigvee_i x_i s_i = x\cdot s.
$$

So the [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani]] routine recovers $x$ with one query.

For $|x|\le k$, sample a random subset $S$ by including each remaining index with probability about $1/k$. With constant probability, $S$ contains exactly one defective item. On the restricted universe, the OR oracle is an inner-product oracle, so one quantum query identifies the defective.

If the exact weight is unknown, try guesses

$$
1,2,4,\ldots,k.
$$

A complement query checks whether all defectives have been found, making the algorithm Las Vegas.

## When to reach for it

- Sparse OR-type identification problems.
- Quantum group testing and restricted junta learning.
- Any oracle where the nonlinear OR collapses to a linear form under a one-solution promise.

## Complexity

Expected query complexity

$$
O(k\log k)
$$

when $|x|\le k$, independent of the ambient universe size $n$.

## Caveat

The known upper bound is not tight. The same paper only proves $\Omega(\sqrt k)$, and explicitly leaves the true quantum query complexity of combinatorial group testing open.

## Related notes

- [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]]
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]
- [[Finite-Field Bernstein-Vazirani via Trace Characters]]
