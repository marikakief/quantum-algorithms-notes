# Montgomery Multiplication as a Quantum Workspace Tradeoff

> **Source:** Roetteler, Naehrig, Svore, and Lauter, arXiv:1706.06752
> **Tags:** #trick #quantum-arithmetic #Montgomery #modular-multiplication #Toffoli

## What it does

Uses Montgomery representation to cut modular-multiplication Toffoli count when the surrounding circuit already has enough workspace.

## The trick

Represent a residue $a\bmod p$ as

$$
aR\bmod p,
\qquad R=2^n>p.
$$

A Montgomery reduction maps

$$
c\longmapsto cR^{-1}\bmod p,
$$

so multiplying two Montgomery residues gives

$$
(aR)(bR)R^{-1}\equiv abR\pmod p,
$$

which stays in Montgomery form.

In a reversible circuit, the gain is that the usual round-by-round modular reductions in double-and-add multiplication are replaced by parity-controlled add-$p$ steps and binary shifts. The Roetteler--Naehrig--Svore--Lauter circuit stores one bit $m_i$ per round recording whether the odd modulus $p$ was added to make the intermediate value even. After copying the product to an output register, it runs the Montgomery multiplication backward to erase the $m_i$ bits.

For an $n$-bit modulus, their comparison is:

$$
\begin{aligned}
T_{\text{dbl/add}}(n)&\approx 32n^2\log_2 n-59.4n^2,\\
T_{\text{Mont}}(n)&\approx 16n^2\log_2 n-26.3n^2.
\end{aligned}
$$

The Montgomery version uses more qubits:

$$
3n+2\quad\text{vs.}\quad 5n+4.
$$

The trick works in their elliptic-curve point adder because [[Fixed-Round Reversible Montgomery-Kaliski Inversion]] already forces a large work register, so the extra Montgomery workspace does not increase the total point-addition qubit count.

## When to reach for it

- Modular arithmetic where multiplication count matters and spare work qubits are already present.
- Reversible elliptic-curve circuits dominated by inversion workspace.
- Resource estimates where Toffoli count is the main logical-level metric.

## Complexity

In [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]], Montgomery multiplication costs

$$
5n+4\text{ qubits},\qquad 16n^2\log_2 n-26.3n^2\text{ Toffolis}.
$$

Montgomery squaring costs

$$
4n+5\text{ qubits},\qquad 16n^2\log_2 n-26.3n^2\text{ Toffolis}.
$$

## Caveat

This is not a free win. If the parent algorithm does not already have idle workspace, the additional $\sim 2n$ qubits may be a bad trade. The copy-out-and-reverse cleanup also doubles the forward arithmetic pattern needed to erase the per-round branch bits.

## Related notes

- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]
- [[Fixed-Round Reversible Montgomery-Kaliski Inversion]]
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]]
- [[Dirty Ancilla Constant Addition]]
