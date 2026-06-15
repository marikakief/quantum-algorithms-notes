# Hash-Minima Distinct-Count Estimation with Quantum Smallest-Values Search

> **Source:** Montanaro, arXiv:1505.00113; Bar-Yossef et al. distinct-elements estimator
> **Tags:** #trick #frequency-moments #distinct-elements #hashing #minimum-finding

## What it does

Estimates the number of distinct stream values $F_0$ by finding the smallest distinct hash values quantumly.

## The trick

Pick a pairwise independent hash function

$$
h:[m]\to[M],\qquad M=m^3.
$$

If $v$ is the $d$th smallest distinct value among $h(a_1),\ldots,h(a_n)$, then

$$
\widetilde F_0=dM/v
$$

estimates $F_0$, for

$$
d=\Theta(1/\epsilon^2).
$$

Classically, finding these distinct minima costs linear scanning. Quantumly, use the D\"urr--Heiligman--H\o yer--Mhalla smallest-values algorithm to find the $d$ smallest hash values of different types in

$$
O(\sqrt{dn})=O(\sqrt n/\epsilon)
$$

queries.

## When to reach for it

- Distinct-count estimation with random access to the data.
- Problems where a classical estimator depends on the lowest order statistics of a hash function.
- Turning an $O(n)$ scan for sampled minima into a Grover/minimum-finding style routine.

## Complexity

For frequency moment $F_0$, query complexity is $O(\sqrt n/\epsilon)$ and this is tight by a threshold-function reduction.

## Caveat

The estimator still needs $d=\Theta(1/\epsilon^2)$ order statistics; the quantum gain is in finding them, not in improving the estimator's sampling variance.

## Related notes

- [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
- [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]]
