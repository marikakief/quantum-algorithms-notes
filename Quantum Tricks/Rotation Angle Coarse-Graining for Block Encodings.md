# Rotation Angle Coarse-Graining for Block Encodings

> **Source:** Lee, Berry, Gidney, Huggins, McClean, Wiebe, Babbush, arXiv:2011.03494
> **Tags:** #trick #block-encoding #rotation-synthesis #error-budget #qubitization #circuit-optimization

## What it does

Reduces the cost of [[QROM-Loaded Givens Rotation Networks|QROM-loaded basis rotations]] in qubitization-based chemistry algorithms by rounding rotation angles to fewer bits of precision. The induced error is bounded analytically and folded into the Hamiltonian approximation error budget, creating a smooth precision-cost tradeoff.

## The trick

In [[THC Non-Orthogonal Diagonal Coulomb Representation|THC qubitization]] and double factorization, the SELECT oracle loads [[Givens Rotation Slater Determinant Preparation|Givens rotation]] angles from [[QROM (Quantum Read-Only Memory)|QROM]]. Each angle is stored as a $b_\theta$-bit fixed-point number. The QROM data size scales as $O(\Gamma \cdot b_\theta)$ — so $b_\theta$ directly affects both the QROM load cost ($O(\sqrt{\Gamma \cdot b_\theta})$ Toffolis) and the rotation synthesis cost ($O(b_\theta)$ T gates per rotation).

**The coarse-graining idea:** Instead of storing angles to full precision, round each angle to $b_\theta$ bits. Each rounded angle introduces a small rotation error $\delta\theta \leq 2^{-b_\theta}\pi$. These errors propagate into the block-encoded Hamiltonian as an additive perturbation $\delta H$ with operator norm bounded by:

$$\|\delta H\| \leq O(N \cdot 2^{-b_\theta} \cdot \lambda_\zeta)$$

To keep $\|\delta H\| \leq \varepsilon_{\rm rot}$, set:

$$b_\theta = O\left(\log\frac{N\lambda_\zeta}{\varepsilon_{\rm rot}}\right)$$

With $\varepsilon_{\rm rot}$ allocated as a fraction of the total precision budget $\varepsilon$, this gives $b_\theta = O(\log(N\lambda_\zeta/\varepsilon))$ bits — logarithmic in all parameters. The QROM and rotation synthesis costs then scale as $\widetilde{O}(1)$ per angle, preserving the overall $\widetilde{O}(N)$ per-step complexity.

**Practical values:** For FeMoCo at chemical accuracy ($\varepsilon = 1.6$ mHa), $b_\theta = 16$ bits for the Reiher Hamiltonian and $b_\theta = 20$ bits for the Li Hamiltonian suffice.

## When to reach for it

- Any block-encoding construction that involves QROM-loaded rotation angles (THC, double factorization, or future tensor decomposition methods)
- When QROM data size is a bottleneck — coarse-graining can reduce data by $2$–$4\times$
- Applicable to von Burg et al.'s double factorization algorithm too (the paper notes this would reduce their costs)

## Complexity

- Saves $O(\sqrt{\Gamma \cdot \Delta b_\theta})$ Toffolis per step (where $\Delta b_\theta$ is the number of bits saved)
- The "cost" is consuming part of the error budget for rotation imprecision
- Optimal $b_\theta$ is logarithmic in $N/\varepsilon$, so asymptotically this costs nothing

## Caveat

The error bound on $\|\delta H\|$ is worst-case over all possible angle rounding patterns. In practice the actual error may be much smaller (cancellations), but using the worst-case bound is the safe choice. If the error budget is very tight (e.g., sub-milliHartree precision on large systems), $b_\theta$ grows and the savings diminish.

## Related notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[QROM-Loaded Givens Rotation Networks]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
