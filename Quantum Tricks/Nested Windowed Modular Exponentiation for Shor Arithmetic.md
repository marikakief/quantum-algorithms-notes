# Nested Windowed Modular Exponentiation for Shor Arithmetic

> **Source:** Gidney and Ekerå, arXiv:1905.09749
> **Tags:** #trick #quantum-arithmetic #Shor #factoring #resource-estimation

## What it does

Turns the modular-exponentiation core of Shor-style algorithms into a long sequence of QROM-loaded additions by windowing both exponent bits and multiplicand bits.

## The trick

Start from modular exponentiation decomposed into controlled multiplications, and each multiplication decomposed into controlled additions by classically known constants. Then window at two nested levels:

1. **Exponent window.** Process $c_{\exp}$ exponent bits at once. Instead of $c_{\exp}$ separate controlled multiplications, precompute the $2^{c_{\exp}}$ possible constants and use the exponent window as part of a lookup address.
2. **Multiplication window.** Process $c_{\mathrm{mul}}$ factor bits at once. Instead of $c_{\mathrm{mul}}$ separate controlled additions, precompute the $2^{c_{\mathrm{mul}}}$ possible addends and use the factor window as the other part of the lookup address.

Each fused step is a **lookup addition**:

$$
\text{address }(e_{\mathrm{win}},x_{\mathrm{win}})
\xrightarrow{\mathrm{QROM}}
\text{addend}
\xrightarrow{+}
\text{target}
\xrightarrow{\mathrm{measure/uncompute}}
\text{clean}.
$$

In Gidney--Ekerå, this sits inside [[Coset Representation for Approximate Modular Arithmetic]], so the addition can be non-modular, and inside [[Oblivious Carry Runway|oblivious carry runways]], so long additions are split into pieces.

Baseline: without windowing, modular exponentiation applies $n_e$ controlled modular multiplications, each decomposed into controlled additions. Windowing turns many of those controlled additions into fewer lookup additions whose cost depends on QROM, measurement-based uncomputation, and the coset/carry-runway arithmetic model.

In the Gidney--Ekerå cost model, for input size $n$, exponent length $n_e$, exponent window $c_{\exp}$, multiplication window $c_{\mathrm{mul}}$, and runway separation $c_{\mathrm{sep}}$, the lookup-addition count is

$$
\operatorname{LookupAdditionCount}(n,n_e)
= \frac{2nn_e}{c_{\exp}c_{\mathrm{mul}}}\frac{c_{\mathrm{sep}}+1}{c_{\mathrm{sep}}}
+ O\!\left(\frac{n_e\log n}{c_{\exp}c_{\mathrm{mul}}}\right).
$$

## When to reach for it

- Shor-style arithmetic where one operand is quantum and the other is a classically known constant.
- Resource estimates where QROM lookups are cheaper than repeated controlled additions.
- Settings where semi-classical QFT lets exponent qubits be recycled, so only the current exponent window must be live.

## Complexity

With the paper's Cuccaro-style adder, QROM lookup, padding, and measurement-uncomputation conventions, each lookup addition costs roughly

$$
2n + O\!\left(\frac{n c_{\mathrm{pad}}}{c_{\mathrm{sep}}}\right) + 2^{c_{\exp}+c_{\mathrm{mul}}}
$$

Toffoli gates before small uncomputation terms. Gidney--Ekerå's practical settings $c_{\exp},c_{\mathrm{mul}}\in\{4,5,6\}$ keep the lookup table small while cutting the number of arithmetic steps by about $c_{\exp}c_{\mathrm{mul}}$.

## Caveat

The lookup cost grows exponentially in the total window size. Once carry-runway additions become faster, the optimal windows shrink because QROM lookup becomes a larger fraction of the runtime. This is why the best practical windows are small constants, not $\Theta(\log n)$ in the physical-resource optimiser.

## Related notes

- [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Measurement-Based QROM Uncomputation]]
- [[Coset Representation for Approximate Modular Arithmetic]]
- [[Oblivious Carry Runway]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
