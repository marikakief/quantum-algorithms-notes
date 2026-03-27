
> **Source:** Rubin, Berry, Kononov, Malone, Khattar, White, Lee, Neven, Babbush, Baczewski, arXiv:2308.12352
> **Tags:** #trick #arithmetic #product-formula #QROM #inverse-square-root #function-evaluation

## What it does

Computes $b/\sqrt{x}$ on a quantum computer to $\sim 10^{-9}$ relative error using a combination of [[QROM (Quantum Read-Only Memory)|QROM]]-loaded polynomial interpolation and one Newton-Raphson step, with total Toffoli cost $\sim 2137 + 4n^2 + 19n$ (where $n$ is the number of bits per spatial direction).

## The trick

Pure Newton-Raphson (Jones et al. 2012) only converges when the initial guess is close to the true answer — accurate to one part in $2^{32}$ over a range of less than $5\times$ in the argument. Straight QROM lookup would need enormous tables for high precision. The hybrid approach splits the work:

**Step 1 — Variable-spacing QROM interpolation (15 bits):** Partition the argument range into intervals of the form $[2^m, (3/2) \cdot 2^m]$ and $[(3/2) \cdot 2^m, 2^{m+1}]$. Within each interval, evaluate a cubic polynomial

$$\frac{1}{\sqrt{x}} \approx a_0 - a_1(x - x_0) + a_2(x - x_0)^2 - a_3(x - x_0)^3$$

using Horner's rule: $a_0 - (x-x_0)\{a_1 - (x-x_0)[a_2 - a_3(x-x_0)]\}$. Only 3 multiplications needed. The coefficients $a_i$ are pre-optimized numerically and loaded via [[Adaptive QROM for Function Evaluation|variable-spacing QROM]] (cost: $4n + 2$ Toffolis). The subtraction $x - x_0$ is free (Clifford gate for removing a leading bit).

The interpolation coefficients need only 15 bits of precision because Newton-Raphson will refine further.

**Step 2 — Generalized Newton-Raphson (full precision):** Apply one iteration

$$y \mapsto \frac{1}{2}y(2 + b^2 - y^2 x + \delta)$$

where $\delta \sim 5 \times 10^{-9}$ is a pre-optimized correction term. This computes $b/\sqrt{x}$ directly, folding the multiplication by $b$ into the iteration. The constant $b$ is the product-formula time-step coefficient — absorbing it here saves one full multiplication per Coulomb pair.

**Bit-shifting for small $x$:** Strip pairs of leading zeros from $x$ (cost: $n(n+1)$ Toffolis), do the QROM interpolation on the normalized value, then shift back the result (cost: $15n$ Toffolis). This avoids extended-precision arithmetic that would otherwise be needed when $1/\sqrt{x}$ is large.

**Uncomputation:** Phase by each individual pairwise Coulomb potential and uncompute the arithmetic with Cliffords by retaining $\sim 2000$ ancilla qubits. This avoids the much more expensive option of computing the full sum before phasing.

## When to reach for it

- Any high-order [[Trotterized Time Evolution for Chemistry|product formula]] simulation in a real-space or dual basis where Coulomb interactions require $1/\sqrt{x}$ or $1/r$ evaluations
- Situations where you need $b \cdot f(x)$ for a known constant $b$ — fold $b$ into the Newton-Raphson initialization
- Function evaluation where you need $>20$ bits of precision but QROM table size is prohibitive

## Complexity

For $n = 6$ bits per spatial direction (typical for plane-wave chemistry at $\sim 2000$ eV cutoff):
- Sum of squares: $3n^2 - n - 1 = 101$ Toffolis
- QROM: $4n + 2 = 26$ Toffolis
- Interpolation (3 multiplications at 15 bits): $\sim 675$ Toffolis
- Newton-Raphson (square + 2 multiplications at 24 bits + subtraction): $\sim 1393$ Toffolis
- Bit shifting: $n(n+1) + 15n = 132$ Toffolis
- Total: $\sim 2395$ Toffolis per electron pair

For comparison, a full [[product formula]] step over $\eta(\eta-1)/2$ pairs costs $\sim 2395 \cdot \eta(\eta-1)/2$ Toffolis, dominated by this subroutine.

## Caveat

The numerically optimized interpolation coefficients and $\delta$ correction are specific to $1/\sqrt{x}$. Extending to other functions (e.g., $\log$, $\exp$) would require re-optimization of the polynomial coefficients. The variable-spacing QROM assumes the argument range spans many orders of magnitude (powers of 2) — for narrow-range arguments, a uniform QROM might be simpler.

## Related notes
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Adaptive QROM for Function Evaluation]]
- [[QROM Interpolation for Negative Exponential]]
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[Trotterized Time Evolution for Chemistry]]
