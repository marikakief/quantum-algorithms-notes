# Rowsum Phase-Tracking in Pauli Group Arithmetic

> **Source:** Aaronson & Gottesman, arXiv:quant-ph/0406196 (2004)
> **Tags:** #trick #stabilizer #clifford #pauli-group #phase-tracking

## What it does

Multiplies two $n$-qubit Pauli operators and correctly computes the resulting $\pm 1$ phase — including tracking the intermediate $\pm i$ phases that arise from individual single-qubit Pauli products — using only integer arithmetic modulo 4.

## The trick

A Pauli on $n$ qubits is stored as a bit row $(x_{1},z_{1},\ldots,x_{n},z_{n})$ plus a phase bit $r \in \{0,1\}$ (for $\pm 1$). The $\pm i$ phases in $XY = iZ$, $YZ = iX$, $ZX = iY$ (and reverses for $-i$) must be tracked through multiplication to give the final $\pm 1$ sign.

Define $g(x_1,z_1,x_2,z_2) \in \{-1,0,1\}$ as the exponent $e$ such that the single-qubit Pauli product contributes $i^e$ to the phase. The $g$-table:

| $(x_1,z_1)$ | $(x_2,z_2)$ | $g$ |
|---|---|---|
| $(0,0)$: $I$ | any | $0$ |
| any | $(0,0)$: $I$ | $0$ |
| $(1,0)$: $X$ | $(1,1)$: $Y$ | $1$ |
| $(1,0)$: $X$ | $(0,1)$: $Z$ | $-1$ |
| $(1,1)$: $Y$ | $(0,1)$: $Z$ | $1$ |
| $(1,1)$: $Y$ | $(1,0)$: $X$ | $-1$ |
| $(0,1)$: $Z$ | $(1,0)$: $X$ | $1$ |
| $(0,1)$: $Z$ | $(1,1)$: $Y$ | $-1$ |
| (equal non-identity) | (equal non-identity) | $0$ |

**rowsum$(h, i)$:** Replace row $h$ by $R_h \cdot R_i$:
$$S = \Bigl(2r_h + 2r_i + \sum_{j=1}^{n} g(x_{ij}, z_{ij}, x_{hj}, z_{hj})\Bigr) \bmod 4$$
- $S = 0$: set $r_h = 0$.
- $S = 2$: set $r_h = 1$.
- $S$ is always 0 or 2 for valid stabilizer-group elements (the accumulated $i$-powers always cancel to $\pm 1$).

Then: $x_{hj} \leftarrow x_{hj} \oplus x_{ij}$, $z_{hj} \leftarrow z_{hj} \oplus z_{ij}$ for all $j$.

## When to reach for it

- Any code simulating Clifford circuits or stabilizer states needs this (or an equivalent).
- Tracking $\pm 1$ phases through sequences of Pauli multiplications in error-correction decoders.
- Implementing stabilizer group membership tests without converting to matrix form.
- Can also be used in randomised benchmarking simulators, shadow tomography implementations, etc.

## Complexity

$O(n)$ per rowsum call ($n$ single-qubit $g$ lookups + phase accumulation).

## Caveat

The modulo-4 trick works because stabilizer group elements always have $\pm 1$ phase; $\pm i$ phases appear only as intermediates from individual qubit factors and always cancel. If you need to track $\pm i$ overall phases (e.g., for Pauli frames in fault-tolerant decoders), you need to store a 2-bit phase instead of 1-bit. The representation in this paper intentionally restricts to the stabilizer group where $r \in \{0,1\}$ suffices.

## Related notes
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Destabilizer Tableau for Stabilizer Simulation]]
