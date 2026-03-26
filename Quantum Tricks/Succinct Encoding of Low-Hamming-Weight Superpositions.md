# Succinct Encoding of Low-Hamming-Weight Superpositions

> **Source:** Berry, Cleve, Gharibian, arXiv:1211.4637
> **Tags:** #trick #compression #encoding #query-complexity

## What it does

Represents an $m$-qubit quantum state whose amplitude concentrates on Hamming weight $\leq k$ using only $(k+1)$ registers of $\lceil\log(m+1)\rceil$ qubits — exponentially fewer than $m$.

## The trick

For an $m$-bit string $x = 0^{s_1}10^{s_2}1\ldots 0^{s_h}10^t$ with $h = |x| \leq k$, define the encoding:

$$C_m^k|x\rangle = |s_1, s_2, \ldots, s_h, \underbrace{m, \ldots, m}_{k+1-h}\rangle$$

Each register stores the gap between consecutive ones (or $m$ for unused slots). The encoded state lives in $(\mathbb{C}^{m+1})^{\otimes(k+1)}$, which has dimension $(m+1)^{k+1}$ — polynomial in $m$ when $k$ is fixed.

The state $(\alpha|0\rangle + \beta|1\rangle)^{\otimes m}$ with $\beta = \Theta(1/\sqrt{m})$ has amplitude $\alpha^{m-|x|}\beta^{|x|}$ on $|x\rangle$, which decays factorially with Hamming weight. Truncating at $k = \Theta(\log(1/\varepsilon)/\log\log(1/\varepsilon))$ introduces error $O(\varepsilon)$.

Preparation uses the [[Exponential Superposition State Preparation|exponential superposition state]] $|\varphi_q\rangle^{\otimes(k+1)}$ as an intermediate encoding, followed by a cleanup procedure (prefix sums, subtraction, inverse preparation) costing $O(k[\log m + \log\log(1/\varepsilon)])$ gates.

## When to reach for it

- You have a protocol with $m$ ancilla qubits, but most of the amplitude sits on states with few ones
- You need polylogarithmic space but the naive construction uses linear space
- The operations on the logical state can be decomposed into actions that depend only on the positions of the ones (phase gates, controlled unitaries, measurements)

The technique is not limited to the fractional-query setting — any scenario with geometrically decaying amplitudes across Hamming weight shells is a candidate.

## Complexity

- Space: $(k+1)\lceil\log(m+1)\rceil$ qubits instead of $m$
- Preparation: $O(k[\log m + \log\log(1/\varepsilon)])$ gates
- Cleanup (encoding conversion): $O(k \log m)$ gates
- Error: $O(\varepsilon)$ from Hamming weight truncation

## Caveat

Operations that don't respect the low-Hamming-weight structure — e.g., arbitrary unitaries on all $m$ qubits — can't be performed in the compressed form. The technique works because the CGMSY fractional-query protocol's operations (phase gates, oracle calls at positions of ones, measurement) decompose naturally into position-based operations.

Also, the measurement step requires a non-trivial [[Recursive Bisection Measurement|recursive bisection]] protocol; you can't just "measure and decode."

## Related notes

- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]] — source paper
- [[Exponential Superposition State Preparation]] — used to prepare the compressed state
- [[Recursive Bisection Measurement]] — used to measure in the compressed basis
- [[Fractional-Query to Discrete-Query Reduction]] — the broader framework
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes]] — extends this compression to Hamiltonian simulation
- [[Compressed Gap Encoding for Ordered Tuples]] — related compression technique
