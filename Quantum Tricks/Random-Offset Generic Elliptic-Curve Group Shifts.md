# Random-Offset Generic Elliptic-Curve Group Shifts

> **Source:** Proos and Zalka, arXiv:quant-ph/0301141
> **Tags:** #trick #elliptic-curves #quantum-arithmetic #approximation

## What it does

Avoids case distinctions in elliptic-curve point addition by randomising the accumulator so exceptional cases have tiny expected amplitude.

## The trick

For a fixed point $A=(\alpha,\beta)$ on an elliptic curve, the generic affine addition formula for $S+A$ fails when $S=A$, $S=-A$, or $S=O$. A reversible circuit can handle these cases exactly, but the extra branching costs gates and control logic.

Proos and Zalka instead initialise the accumulator not at $O$, but at a random nonzero point $kP$ in the cyclic subgroup. The overall offset only changes Fourier-sampling phases, not the measured relation used to recover the discrete logarithm.

For each fixed shift by $A$, a random accumulator state hits $A$ or $-A$ with probability about $2/q$. Across the $2n$ controlled shifts in the ECDLP circuit, the expected lost fidelity is estimated as

$$
\frac{4n}{q},
$$

which is exponentially small for cryptographic-size $q$.

## When to reach for it

- Large finite groups where a reversible group operation has rare exceptional cases.
- Fourier-sampling algorithms where a global group offset changes only unobserved phases.
- Early-stage resource estimates where exact branching logic would obscure the main cost.

## Complexity

Saves the exact exceptional-case circuitry for every elliptic-curve addition. The cost is a small algorithmic approximation error of order $n/q$ for $2n$ shifts.

## Caveat

This is not an exact group-addition circuit. It is safe only when the failure amplitude is negligible for the intended parameter regime and the algorithm tolerates the corresponding fidelity loss.

## Related notes

- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]]
- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]
- [[Slope-Uncomputed In-Place Elliptic-Curve Shifts]]
- [[Optimistic Quantum Circuits]]
