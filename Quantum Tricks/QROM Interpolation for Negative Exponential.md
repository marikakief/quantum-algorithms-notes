# QROM Interpolation for Negative Exponential

> **Source:** Berry, Rubin, Babbush et al., arXiv:2312.07654; also Sanders, Berry, Babbush et al., arXiv:1902.10673
> **Tags:** #trick #circuit-primitive #function-evaluation #QROM #interpolation #exponential #arithmetic

## What it does

Efficiently computes $e^{-z}$ in a quantum register using a combination of base conversion, [[QROM (Quantum Read-Only Memory)|QROM]]-loaded polynomial interpolation on the fractional part, and controlled bit shifts for the integer part. Achieves relative error $\delta_f$ with QROM cost scaling as $(1/\delta_f)^{1/3}$ for quadratic interpolation.

## The trick

Computing $e^{-z}$ directly with a Taylor series in a quantum register is expensive — many multiplications, each costing $O(b^2)$ Toffolis for $b$-bit precision. Instead:

1. **Convert to base 2:** Multiply $z$ by $1/\ln 2$ to get $z' = z/\ln 2$, so we need $2^{-z'}$. Cost: $\sim b^2/4$ Toffolis (since about half the bits of $1/\ln 2$ are zero).

2. **Split $z'$** into an integer part and a fractional part. The fractional part is further split into leading bits (used as a QROM address) and remaining bits (used as the interpolation variable $\delta$).

3. **QROM on the leading fractional bits** outputs polynomial coefficients $(a_0, a_1, a_2)$ for a local interpolating polynomial (Chebyshev nodes for near-optimal approximation).

4. **Evaluate the polynomial** on the remaining fractional bits: $a_0 + a_1 \delta(a_2 + \delta)$ for quadratic interpolation. This costs one or two multiplications ($b^2$ or $2b^2$ Toffolis).

5. **Controlled bit shift** by the integer part. Since $2^{-z'} = 2^{-\lfloor z'\rfloor} \cdot 2^{-\{z'\}}$, shifting the interpolated fractional result by the integer part gives the full answer. Cost: $\sim b^2/2$ Toffolis.

The total Toffoli cost is:
- **Linear interpolation:** $(3/4)b^2 + P$ where $P$ is the number of QROM interpolation points (e.g., 256 for $10^{-6}$ relative error)
- **Quadratic interpolation:** $(11/4)b^2 + P$ where $P \sim 128$ for $10^{-9}$ relative error

The QROM cost $P$ scales as $(\ln 2)/(192\,\delta_f)^{1/3}$ for quadratic interpolation — the cube root means you can improve precision by two orders of magnitude at the cost of only $\sim 10\times$ more QROM entries.

## When to reach for it

- Any block encoding that requires a negative exponential (Gaussians, screened potentials, thermal states)
- When the function argument varies continuously and you can't afford to tabulate the entire range via QROM
- Particularly useful when the same exponential needs to be evaluated for many different terms (as in the GTH pseudopotential, where local and nonlocal parts share the exponential)
- Any situation where a smooth, bounded function needs to be computed coherently with arbitrary precision

## Complexity

| Interpolation order | QROM points (for $\delta_f$) | Arithmetic (Toffolis) | Total (for $b = 20$, $\delta_f = 10^{-6}$) |
|---|---|---|---|
| Linear | $\ln 2 / \sqrt{8\delta_f}$ | $(3/4)b^2$ | ~556 |
| Quadratic | $(\ln 2/(192\delta_f))^{1/3}$ | $(11/4)b^2$ | ~1,228 |

For $b = 20$ and $\delta_f = 10^{-6}$: total ~5,000 Toffolis as part of a pseudopotential block encoding costing ~20,000–50,000 total.

## Caveat

The base-2 conversion step adds an extra multiplication ($b^2/4$), which wouldn't be needed if you could interpolate $e^{-z}$ directly with comparable efficiency. But the bit-shift trick for the integer part saves far more than this costs, because it avoids computing a power-of-$e$ multiplicative factor entirely.

The method assumes the function is smooth and bounded on $[0, z_{\max}]$. For functions with singularities or rapid oscillations, the interpolation error bounds don't hold and you'd need many more interpolation points.

## Related notes
- [[QROM (Quantum Read-Only Memory)]] — The data loading primitive used for interpolation coefficients
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — Primary application: GTH pseudopotential evaluation
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — Error analysis for QROM interpolation
- [[Phase Gradient State for Controlled Rotations]] — The bit-shift technique is related to phase gradient methods
