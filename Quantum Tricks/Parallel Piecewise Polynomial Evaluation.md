# Parallel Piecewise Polynomial Evaluation

> **Source:** Häner, Roetteler, Svore, arXiv:1805.12445
> **Tags:** #trick #quantum-arithmetic #function-evaluation #piecewise-polynomial #Horner-scheme

## What it does
Evaluates a piecewise polynomial approximation of a function $f(x)$ on a quantum register, with cost that scales as $O(\log M)$ overhead per Horner step rather than $O(M)$ for $M$ subintervals.

## The trick
Approximating a function over a domain $\Omega$ to precision $\varepsilon$ often requires either a very high-degree polynomial or many low-degree polynomials on subintervals. The naive quantum approach — looping over $M$ subintervals and conditionally evaluating — costs $O(M)$ polynomial evaluations.

The parallel scheme introduces a $\lceil \log_2 M \rceil$-qubit label register $|l\rangle$ encoding which subinterval the input belongs to:

1. **Label computation.** Compare $x$ against each interval boundary using a comparator (e.g., the CARRY circuit from [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner-Roetteler-Svore (2017)]]). The label register is incremented for each passed boundary. Cost: $4Mn$ Toffolis.

2. **Parallel Horner iteration.** At each step $i$ of the Horner scheme, a NEXTa gate loads the correct coefficient $a_{l,i}$ conditioned on $|l\rangle$:
$$\sum_l |l\rangle|a_{l,i-1}\rangle \mapsto \sum_l |l\rangle|a_{l,i}\rangle$$
This is a multi-controlled NOT with $\lceil \log_2 M \rceil$ controls, costing $4(\lceil \log_2 M \rceil - 2)$ Toffolis using dirty ancillae. The multiplication and addition steps are identical to single-polynomial Horner.

3. **Space/time tradeoff.** The $d+1$ intermediate iterates can be reduced using [[Pebble Game Space Reduction for Recursive Circuits|Bennett pebbling]] on the linear dependency chain.

The overhead from parallelism per degree-$d$ polynomial evaluation is $2Md(4\lceil \log_2 M \rceil - 8) + 4Mn$ Toffolis and $\lceil \log_2 M \rceil + 1$ extra qubits. This is small relative to the $O(n^2 d)$ cost of the multiplications.

## When to reach for it
- Evaluating smooth functions ($\tanh$, Gaussian, $\sin$, $\cos$, $e^{-x}$) on quantum registers where a single polynomial would need very high degree.
- Any quantum oracle compilation where you need to approximate a classical function to precision $\varepsilon$ using basis-state arithmetic.
- When combined with the Remez algorithm for automatic partitioning, this gives an end-to-end compiler from function specification to circuit.

## Complexity
$$T_{\text{pp}}(n, d, p, M) = \tfrac{3}{2}n^2 d + 3npd + \tfrac{7}{2}nd - 3p^2 d + 3pd - d + 2Md(4\lceil \log_2 M \rceil - 8) + 4Mn$$

Qubits: $(d+1)n + \lceil \log_2 M \rceil + 1$.

Typical: $\tanh$ to $10^{-7}$ error with degree-4 polynomials on 23 subintervals costs 23,095 Toffolis (compute only) on 205 qubits.

## Caveat
The cost is dominated by the $O(n^2 d)$ multiplication term, not the parallelism overhead. For high precision ($n > 50$), [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes|Karatsuba multiplication]] could reduce this. For low-precision or heuristic applications, [[Adaptive QROM for Function Evaluation|QROM-based interpolation]] may be cheaper. For state preparation specifically, [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|inequality-test approaches]] avoid arithmetic entirely.

## Related notes
- [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]]
- [[Remez-Based Automatic Oracle Compilation]]
- [[Pebble Game Space Reduction for Recursive Circuits]]
- [[Adaptive QROM for Function Evaluation]]
- [[Reversible Computation via Compute-Copy-Uncompute]]
