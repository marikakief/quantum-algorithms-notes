# Reversible Newton-Raphson with Tuned Initial Guess

> **Source:** Häner, Roetteler, Svore, arXiv:1805.12445
> **Tags:** #trick #quantum-arithmetic #Newton-method #inverse-square-root #function-evaluation

## What it does
Computes $1/\sqrt{a}$ (and by extension $\sqrt{a}$ and $\arcsin(a)$) on a quantum register using reversible Newton-Raphson iteration with a carefully tuned initial guess that saves one iteration compared to the raw approach.

## The trick
The Newton iteration for inverse square root is $x_{n+1} = x_n(1.5 - ax_n^2/2)$, which converges quadratically given a good starting point.

**The initial guess (adapted from Quake III Arena):**

1. Find the leading 1-bit position in $a$ by scanning from MSB to LSB with a flag qubit. This gives $\lfloor \log_2 a \rfloor$ and costs $2n$ Toffolis.

2. The crude guess is $\tilde{x}_0 = 2^{-\lfloor \log_2 a \rfloor / 2}$, which gets the exponent right but ignores the mantissa. Fold the first Newton step into the initialisation:

$$x_0 = 2^k \left(C(k) - \frac{a \cdot 2^{3k}}{2}\right)$$

where $k = \lfloor(p - \lfloor \log_2 a \rfloor)/2\rfloor$.

3. **The tuning:** Instead of using $C(k) = 1.5$ universally, distinguish three cases:

$$C(k) = \begin{cases} 1.613 & k < 0 \\ 1.5 & k = 0 \\ 1.62 & k > 0 \end{cases}$$

This costs only a few extra CNOT gates (controlled on the sign of $k$) but eliminates the error peaks near powers of two that appear in the untuned version. The result: one fewer Newton iteration for the same target precision.

**The reversible Newton step:** Each iteration stores its result in a new register:

1. Square $x_n$ (one register)
2. Multiply $x_n^2$ by $a$ (second register)
3. Subtract from 1.5 (third register)
4. Multiply by $x_n$ to get $x_{n+1}$ (output register)
5. Uncompute the three intermediates in reverse order

Cost per iteration: $5T_{\text{mul}} + 2T_{\text{add}} = \frac{15}{2}n^2 + O(np)$ Toffolis. Space: 3 temporary $n$-bit registers per iteration, plus the iterate register.

## When to reach for it
- Computing $1/\sqrt{x}$, $\sqrt{x}$, or $\arcsin(x)$ on quantum registers at moderate-to-high precision.
- Quantum chemistry oracles needing Coulomb potential evaluation ($1/r$).
- Any quantum algorithm requiring non-polynomial function evaluation where QROM lookup alone is insufficient.

## Complexity
$$T_{\text{invsqrt}}(n, m, p) = n^2\!\left(\tfrac{15}{2}m + 3\right) + 15npm + O(nm)$$

Qubits: $n(m+4)$. Practical: $m = 3$ Newton iterations at $n = 35$ bits gives $\sim 10^{-10}$ error; without tuning, $m = 4$ would be needed ($\sim 25\%$ more cost).

## Caveat
The $O(mn^2)$ Toffoli cost per evaluation is high. For applications where $1/\sqrt{x}$ only needs to be evaluated approximately, [[QROM + Newton Iteration for Function Evaluation|QROM + Newton]] (using a precomputed table for the initial guess) is cheaper. The fixed-point representation wastes bits on inputs far from the overflow boundary. The three-way case distinction on $C(k)$ is tuned for uniform error across $[1/N, 5]$ and may not be optimal for other domains.

## Related notes
- [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]]
- [[Parallel Piecewise Polynomial Evaluation]]
- [[QROM + Newton Iteration for Function Evaluation]]
- [[QROM Interpolation plus Newton-Raphson for Inverse Square Root]]
- [[Reversible Computation via Compute-Copy-Uncompute]]
