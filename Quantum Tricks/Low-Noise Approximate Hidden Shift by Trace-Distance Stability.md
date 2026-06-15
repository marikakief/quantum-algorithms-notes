# Low-Noise Approximate Hidden Shift by Trace-Distance Stability

> **Source:** Montanaro, arXiv:1408.1816
> **Tags:** #trick #hidden-shift #trace-distance #average-case #query-complexity

## What it does

Extends a hidden-shift sieve to very low-noise approximate shifts by bounding how much the mixed input state changes in trace distance.

## The trick

The exact hidden-shift algorithm consumes many copies of a mixed state $\rho$ derived from oracle states

$$
|\phi_r\rangle=\frac{1}{\sqrt2}\big(|0\rangle|f(r)\rangle+|1\rangle|g(r)\rangle\big).
$$

If $f$ and $g$ are changed on an $\epsilon$ fraction of points, the corresponding mixed state $\widetilde\rho$ obeys

$$
\|\widetilde\rho-\rho\|_1\le 2\epsilon.
$$

For an algorithm using $Q$ copies, the distinguishing penalty is $O(Q\epsilon)$. Thus the exact algorithm still works when the mismatch fraction is much smaller than $1/Q$, after union-bounding over the bit-recovery stages.

## When to reach for it

- Query algorithms whose entire input dependence is through copies of a state $\rho$.
- Low-noise hidden-shift or alignment problems where a candidate shift is almost exact.
- Stability proofs where the algorithm need not identify corrupted positions.

## Complexity

For Montanaro's $\mathbb Z_{2^n}^d$ hidden-shift sieve, $Q=O(n2^{\sqrt{(2\log_2 3)dn}})$, and the paper assumes mismatch rate

$$
O\!\left(n^{-2}2^{-\sqrt{(2\log_2 3)dn}}\right).
$$

## Caveat

This handles only very low noise. Constant-noise hidden shift remains a different problem and likely needs new ideas.

## Related notes

- [[Quantum Pattern Matching Fast on Average (Montanaro 2015) — Paper Notes]]
- [[Kuperberg Sieve with Recycled Failures]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
