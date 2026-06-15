# One-Bit Branch Logging for Reversible Binary GCD

> **Source:** Roetteler, Naehrig, Svore, and Lauter, arXiv:1706.06752
> **Tags:** #trick #reversible-computation #binary-GCD #quantum-arithmetic #uncomputation

## What it does

Records a four-branch binary-GCD update with one persistent bit per round by recovering the missing bit from a parity invariant.

## The trick

A reversible implementation of Kaliski's binary-GCD inverse must remember which branch was taken in each round so it can later reverse the update. Naively, four branches need two bits:

1. $u$ even;
2. $u$ odd and $v$ even;
3. $u,v$ odd and $u>v$;
4. $u,v$ odd and $u\le v$.

Roetteler--Naehrig--Svore--Lauter encode the branch as two temporary bits $ab$, with correspondences

$$
10,
\quad 01,
\quad 11,
\quad 00.
$$

Only $b$ is stored persistently as the round bit $m_i$. The other bit $a$ is uncomputed immediately after the branch update.

The reason this is possible is the invariant of Kaliski's algorithm: after each step, exactly one of the coefficient registers $r$ and $s$ is even and the other is odd. More specifically:

- if updated $r$ is even, the branch was either $v$ even or $u,v$ odd with $u\le v$;
- if updated $s$ is even, the branch was either $u$ even or $u,v$ odd with $u>v$.

Together with the stored bit $b=m_i$, this parity information identifies the original branch. So $a$ can be recovered from the live arithmetic state during reversal, rather than stored for all $2n$ rounds.

## When to reach for it

- Reversible versions of classical algorithms with a small finite branch set.
- Branchy arithmetic where a loop invariant distinguishes subsets of cases.
- Circuits where storing two history bits per round would dominate workspace.

## Complexity

For $2n$ rounds of Montgomery--Kaliski inversion, the trick stores $2n$ history bits rather than $4n$. It does not change the asymptotic Toffoli count, but it removes $2n$ persistent qubits from the inverse circuit.

## Caveat

The saved bit must be recoverable from a true invariant of the updated state. If noise, approximation, or later arithmetic breaks the parity invariant before reversal, uncomputation fails. This is a reversible-circuit identity, not a fault-tolerance mechanism.

## Related notes

- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]
- [[Fixed-Round Reversible Montgomery-Kaliski Inversion]]
- [[Desynchronised Reversible Euclidean Algorithm]]
- [[Reversible Computation via Compute-Copy-Uncompute]]
