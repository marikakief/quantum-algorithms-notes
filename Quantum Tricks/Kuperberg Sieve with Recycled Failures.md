# Kuperberg Sieve with Recycled Failures

> **Source:** Montanaro, arXiv:1408.1816; building on Kuperberg, quant-ph/0302112
> **Tags:** #trick #hidden-shift #quantum-sieve #labelled-qubits #dihedral-HSP

## What it does

Improves a Kuperberg-style labelled-qubit sieve by keeping the states produced by failed pair-combinations instead of throwing them away.

## The trick

A hidden-shift query produces labelled qubit states

$$
|\psi_r\rangle=\frac{1}{\sqrt2}\left(|0\rangle+\omega^{r\cdot s}|1\rangle\right).
$$

Combining $|\psi_r\rangle$ and $|\psi_t\rangle$ by a parity measurement yields either

$$
|\psi_{r-t}\rangle \quad\text{or}\quad |\psi_{r+t}\rangle
$$

with probability $1/2$ each. In the usual sieve, only the outcome that zeros a chosen block of label bits is kept. Montanaro observes that if $r$ and $t$ agree on those bits, then both $r-t$ and $r+t$ keep the same zeroed-bit structure. The nominal failure outcome can therefore remain in the bin and be paired again.

This changes the effective survival fraction per stage from about $1/4$ to about $1/3$.

## When to reach for it

- Labelled-state sieves where both measurement outcomes preserve the stage invariant.
- Hidden-shift or HSP algorithms where a failed combination is still structured enough to recycle.

## Complexity

For hidden shift over $\mathbb Z_{2^n}^d$, the resulting state count is

$$
O\!\left(n2^{\sqrt{(2\log_2 3)dn}}\right),
$$

with the constant in the exponent coming from the $1/3$ survival analysis.

## Caveat

This improves constants, not the asymptotic type: the algorithm is still subexponential. It also needs enough memory to store a large pool of labelled qubits.

## Related notes

- [[Quantum Pattern Matching Fast on Average (Montanaro 2015) — Paper Notes]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Quantum Sieve for Labelled Qubits]]
