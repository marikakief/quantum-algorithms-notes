# Fixed-Round Reversible Montgomery-Kaliski Inversion

> **Source:** Roetteler, Naehrig, Svore, and Lauter, arXiv:1706.06752
> **Tags:** #trick #quantum-arithmetic #modular-inversion #reversible-computation #binary-GCD

## What it does

Turns Kaliski's variable-time Montgomery inverse into a fixed-depth reversible circuit suitable for superposition inputs.

## The trick

Kaliski's Montgomery inverse is a binary-GCD-style algorithm. For an odd prime $p$ and input $x$, it maintains registers

$$
u=p,
\qquad v=x,
\qquad r=0,
\qquad s=1,
$$

with invariant

$$
p=rv+su.
$$

Each iteration applies one branch:

$$
\begin{array}{ll}
 u\text{ even}: & (u,s)\leftarrow (u/2,2s),\\
 v\text{ even}: & (v,r)\leftarrow (v/2,2r),\\
 u>v\text{ odd}: & (u,r,s)\leftarrow ((u-v)/2,r+s,2s),\\
 u\le v\text{ odd}: & (v,r,s)\leftarrow ((v-u)/2,2r,r+s).
\end{array}
$$

The classical algorithm halts after an input-dependent number $k$. A quantum circuit cannot let different basis states stop at different clock times, so Roetteler--Naehrig--Svore--Lauter pad every execution to the worst-case bound of $2n$ rounds for an $n$-bit modulus.

They add:

1. a flag $f$ that says whether the circuit is still in Kaliski mode;
2. a small counter, using $\lceil \log_2 n\rceil+1$ qubits, that advances after termination;
3. one persistent branch bit $m_i$ per round so the round can be reversed later.

After $2n$ rounds, $r$ contains an almost inverse, of the form $-x^{-1}2^k$ in the paper's convention. The circuit converts this to the desired Montgomery inverse $x^{-1}2^n\bmod p$ using controlled modular doublings and a sign flip, copies out the result, then runs the whole fixed-round computation backward to erase all branch bits and work registers.

## When to reach for it

- Reversible modular inversion inside quantum arithmetic.
- Classical algorithms with a known worst-case iteration bound but input-dependent halting.
- Settings where divisions are too expensive and binary shifts/additions are preferable.

## Complexity

For an $n$-bit prime modulus, the inverse circuit in [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]] uses

$$
7n+2\lceil\log_2 n\rceil+9
$$

qubits, of which

$$
5n+2\lceil\log_2 n\rceil+9
$$

are ancillae, and costs

$$
32n^2\log_2 n
$$

Toffoli gates.

## Caveat

The fixed $2n$-round schedule is clean and testable, but space-heavy. It stores $2n$ branch bits and needs an output copy so the computation can be reversed. [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes|Proos--Zalka]] proposed register-sharing ideas that could reduce space, but this paper does not implement them.

## Related notes

- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]
- [[One-Bit Branch Logging for Reversible Binary GCD]]
- [[Montgomery Multiplication as a Quantum Workspace Tradeoff]]
- [[Desynchronised Reversible Euclidean Algorithm]]
- [[Slope-Uncomputed In-Place Elliptic-Curve Shifts]]
