# Oblivious Carry Runway

> **Source:** Craig Gidney, arXiv:1905.08488
> **Tags:** #trick #quantum-arithmetic #approximate #adder #carry-propagation

## What it does

Splits one long $n$-bit addition into independent parallel additions on short segments by inserting carry-absorbing padding qubits, without leaking which-path information about the addition history.

## The trick

In a classical register, you can insert extra "runway" bits at position $p$ so that carries from the low half are absorbed by the runway instead of propagating into the high half. This lets you add to both halves independently. But in a quantum register, a naively initialised runway becomes entangled with the computation — the runway state records whether a carry arrived, leaking information.

The fix: initialise the $m$ runway qubits to $|+\rangle^{\otimes m}$, then subtract the runway value from the high part of the register at position $p$. The encoding is:

$$f((g, c)) = \big((g \bmod 2^p) + 2^p c, \;\; (\lfloor g/2^p \rfloor - c) \bmod 2^{n-p}\big)$$

where $c$ is the coset value (the runway content). Adding $k$ to the encoded register becomes two independent additions: add $k \bmod 2^p$ to the low piece (of length $p + m$) and $\lfloor k/2^p \rfloor$ to the high piece (of length $n - p$).

The runway overflows only when it is storing its maximum value $2^m - 1$ and gets incremented — one bad value out of $2^m$. So the deviation is $\leq 2^{-m}$.

With $r$ evenly spaced runways (spacing $s$), all $r + 1$ pieces can be added in parallel, each with a short ripple-carry adder of depth $O(s + m)$ instead of $O(n)$.

## When to reach for it

- Any algorithm that performs many additions into the same register and where circuit depth (not gate count) is the bottleneck — primarily Shor's modular exponentiation.
- When you want sub-linear-in-$n$ depth addition but can't afford the ancilla overhead of a full carry-lookahead adder on the entire register.
- As an alternative to carry-lookahead when spacetime volume is the optimisation target: the piecewise adder trades a small approximation error for reduced ancilla usage and depth, often winning on volume.
- Karatsuba multiplication: the padding registers in [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes|Parent-Roetteler-Mosca (2017)]] can be replaced by oblivious carry runways, avoiding the uncomputation cost.

## Complexity

For $r$ runways of length $m$ with spacing $s$ on an $n$-bit register, performing $k$ additions using [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro]] ripple-carry on each piece:

| Resource | Cost |
|---|---|
| Measurement depth (total) | $\sim 2(s+m) \cdot k$ |
| Toffoli count (total) | $2(n + mr) \cdot k$ |
| Extra qubits | $mr$ |
| Trace distance from ideal | $\leq 2\sqrt{k \cdot r \cdot 2^{-m}}$ |

## Caveat

- The encoding/decoding cost (preparing $|+\rangle$ runways and subtracting) is a one-time overhead that must be amortised over many additions. For a single addition, an exact adder is cheaper.
- The approximation error grows with the number of additions $k$ and the number of runways $r$. For Shor's algorithm with $k = O(n^2)$ and $r = O(n/s)$, the padding length must be $m = \Omega(\log n)$ to keep total error bounded.
- The runway qubits consume physical space that could otherwise be used for T factories (see [[Ancilla Opportunity Cost Analysis]]).

## Related notes
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]] — source paper
- [[Approximate Encoded Permutation Framework]] — the error-analysis framework
- [[Coset Representation for Approximate Modular Arithmetic]] — the complementary technique for modular arithmetic; typically concatenated with carry runways
- [[Carry-Lookahead Tree for Logarithmic-Depth Addition]] — the exact alternative; wins on depth alone but loses on spacetime volume at practical sizes
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — the T-count-optimised ripple-carry adder used within each piece
- [[Temporary Logical-AND]] — used inside the per-piece ripple-carry adders
- [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]] — uses carry runways to parallelise lookup additions and reduce RSA-2048 spacetime volume
