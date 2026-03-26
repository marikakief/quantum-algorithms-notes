# Recursive Bisection Measurement

> **Source:** Berry, Cleve, Gharibian, arXiv:1211.4637
> **Tags:** #trick #measurement #compression #query-complexity

## What it does

Simulates a complete computational-basis measurement of $m$ logical qubits when only a compressed $O(k\log m)$-qubit representation is available. Produces the same outcome distribution as measuring the uncompressed state, up to controllable error.

## The trick

The problem: you have $m$ logical qubits stored in a [[Succinct Encoding of Low-Hamming-Weight Superpositions|succinct encoding]] and need to perform a projective measurement in the basis $\{R^{\otimes m}|x\rangle : x \in \{0,1\}^m\}$. Decoding to $m$ qubits and measuring would cost $O(m)$ gates.

Instead, recursively bisect:

1. **Test all-zero:** Project onto $|0^m\rangle$ vs. its complement. In compressed form, $C_m^k|0^m\rangle = |m\rangle^{\otimes(k+1)}$ is a computational basis state. Apply $U_m^\dagger$ (inverse of compressed state preparation), measure whether the result is $|m\rangle^{\otimes(k+1)}$, then apply $U_m$.
2. **If non-zero:** Split the $m$-bit string into two halves using Lemma 5.1 ($C_m^k|x_1x_2\rangle \mapsto C_{m/2}^k|x_1\rangle \otimes C_{m/2}^k|x_2\rangle$ in $O(k\log m)$ gates). Recurse on each half independently.
3. **Base case:** When $m_1 = m_2$ (single bit), a non-zero test reveals the bit value directly.

The recursion terminates for Hamming weight $h$ outcomes after at most $1 + 2h\log m$ steps. Since the expected Hamming weight is $O(1)$ (from $\beta^2 \approx 1/(8m)$), the expected cost is $O(\log m)$ steps.

## When to reach for it

- You have a compressed representation of $m$ qubits and need the full measurement outcome
- The outcome distribution concentrates on low Hamming weight
- Direct decompression would blow up the space/gate budget

The technique is specific to encodings with product-form state preparation (the $U_m$ and $U_m^\dagger$ operations must be efficient). It doesn't work for arbitrary compressions.

## Complexity

- Per step: $O(k[\log m + \log\log(1/\varepsilon)])$ gates (for $U_n, U_n^\dagger$) plus $O(k\log m)$ for splitting
- Expected total: $O(\log m)$ steps $\times$ per-step cost
- Worst case (Hamming weight $h$): $O(h\log m)$ steps, capped at $k'\log m$

## Caveat

**Error accumulation.** Each recursive step introduces $O(\varepsilon)$ error (from imprecise $U_n$ and Hamming weight truncation). The naive bound is $O(\varepsilon k'\log m)$. The improved bound (Theorem 5.5) exploits the $O(1)$ expected Hamming weight to get $O(\varepsilon\log m + \varepsilon')$.

The improved bound requires $\varepsilon = O(1/(k'\log m))$. If you need the error much smaller than this, the preparation precision must scale accordingly, increasing gate count.

## Related notes

- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]] — source paper (Algorithm 5.2)
- [[Succinct Encoding of Low-Hamming-Weight Superpositions]] — the encoding being measured
- [[Exponential Superposition State Preparation]] — the preparation procedure whose inverse is used
