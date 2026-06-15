# Average-Case Search Lower Bound by Prefix Rearrangement

> **Source:** Montanaro, arXiv:0908.3066
> **Tags:** #trick #lower-bound #quantum-search #query-complexity #average-case

## What it does

Converts worst-case Grover lower bounds on prefixes into an average-case lower bound for a nonuniform advice distribution.

## The trick

Let a Las Vegas quantum search algorithm have expected query cost $T_A(x)$ when the marked item is $x$. For a domain of size $m$, Grover optimality plus Markov's inequality implies that some item in the domain must have expected cost

$$
\Omega(\sqrt m).
$$

Apply this statement iteratively to prefixes/subsets: among any $m$ possible marked locations, one location must be expensive. This gives a multiset lower bound of the form

$$
T_A(x_i) \gtrsim c\sqrt{i}-1
$$

for some ordering of the inputs.

If the advice probabilities are sorted as $p_1\ge p_2\ge\cdots\ge p_n$, the rearrangement inequality pairs the largest probabilities with the smallest possible costs. Therefore every algorithm has expected cost at least

$$
\sum_{x=1}^n p_x(c\sqrt{x}-1)
=\Omega\!\left(\sum_x p_x\sqrt{x}\right)-1.
$$

Montanaro makes the constant explicit:

$$
Q(\mu)\ge 0.206\sum_x p_x\sqrt{x}-1.
$$

## When to reach for it

- Average-case query lower bounds with a sorted input distribution.
- Problems where every large subset contains a hard input.
- Showing that a natural “search likely cases first” algorithm is optimal up to constants.

## Complexity

For search with known advice distribution $\mu$, the method proves the lower bound matching [[Geometric Grover Search over Advice-Ordered Blocks]] up to constants:

$$
Q(\mu)=\Theta\!\left(\sum_x p_x\sqrt{x}\right).
$$

## Caveat

This is tailored to search-like lower bounds where every subset inherits a Grover-type obstruction. It does not automatically apply to structured problems where hard instances are not evenly spread across prefixes or subsets.

## Related notes

- [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]]
- [[Geometric Grover Search over Advice-Ordered Blocks]]
- [[Hybrid Argument Lower Bound for Quantum Search]]
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]]
