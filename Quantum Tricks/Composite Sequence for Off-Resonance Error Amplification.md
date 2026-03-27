# Composite Sequence for Off-Resonance Error Amplification

> **Source:** Kimmel, Low, Yoder, arXiv:1502.02677 (2015)
> **Tags:** #trick #gate-calibration #composite-gates #error-amplification #QSP-precursor

## What it does

Converts a small, hard-to-measure axis tilt error ($\theta$) in a single-qubit gate into a rotation angle ($\Phi$) that grows linearly with $\theta$, making it accessible to standard [[Iterative Phase Estimation (Kitaev)|phase estimation]].

## The trick

Suppose your $X_{\pi/4}$ gate has both an amplitude error $\epsilon$ and an axis tilt $\theta$ — the rotation axis lies in the $XZ$ plane at angle $\theta$ from the $X$-axis instead of along $X$.

The axis tilt doesn't directly appear in the eigenvalues of $X_{\pi/4}(\epsilon, \theta)^k$, so repeating the gate and measuring won't isolate $\theta$. Instead, construct the composite:

$$U = Z_{\pi/2}\,X_{\pi/4}^4\,Z_{\pi/2}^2\,X_{\pi/4}^4\,Z_{\pi/2}$$

where $Z_{\pi/2}$ is the (already-calibrated) $Z$ rotation. This $U$ is a rotation by angle $\Phi$ about an axis in the $XZ$ plane, where:

$$\sin\!\left(\frac{\Phi}{2}\right) = 2\sin\theta\cos\!\left(\frac{\pi\epsilon}{2}\right)\sqrt{1 - \sin^2\theta\cos^2\!\left(\frac{\pi\epsilon}{2}\right)}$$

For small $\theta$: $\Phi \approx 4\theta\cos(\pi\epsilon/2)$. Now apply $U^k$ and use phase estimation to extract $\Phi$ at the Heisenberg limit — then solve for $\theta$ using the already-estimated $\epsilon$.

The $Z$ rotations in the composite act as "frame changes" that convert the tilt into a measurable rotation angle. The specific structure $Z\cdot X^4\cdot Z^2\cdot X^4\cdot Z$ is chosen so that the $Y$-component of the rotation axis vanishes, keeping the analysis in the $XZ$ plane.

## When to reach for it

- Single-qubit gate calibration when you need to isolate off-resonance (axis) errors from amplitude errors.
- Any setting where a parameter doesn't appear in the eigenvalue spectrum of a single gate but does appear when gates are composed.
- As a design pattern: use known-good gates as "lenses" to convert hidden error parameters into observable ones.
- The general idea — composing gates to amplify specific error channels — reappears in [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|composite gate design]] and is a precursor to [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] polynomial engineering.

## Complexity

- Each application of $U$ uses 4 $Z_{\pi/2}$ gates and 8 $X_{\pi/4}$ gates (12 gates total).
- Phase estimation with $K$ rounds uses $k_j = 2^{j-1}$ repetitions of $U$, so the longest sequence has $12 \cdot 2^{K-1}$ gates.
- Total gate count is $O(N)$ where $N$ is the target precision $1/\sigma(\hat{\theta})$ — Heisenberg-limited.

## Caveat

- Requires the $Z$ gate to already be well-calibrated ($|\alpha| = O(1/k)$ for the $k$th step). The sequential protocol in the paper calibrates $Z$ first, then uses it here.
- Only works when the axis tilt is small enough that the $\Phi$-to-$\theta$ map is approximately linear ($|\theta| \lesssim 36°$). For large $\theta$, the nonlinearity means a Heisenberg-limited estimate of $\Phi$ doesn't directly translate to Heisenberg-limited $\theta$ — bootstrapping or nonparametric methods are needed.
- Specific to single-qubit $SU(2)$ geometry. The construction relies on the Bloch sphere structure where rotations compose predictably.

## Related notes
- [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes]]
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes]] — generalises this composite-gate approach
- [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes]] — related polynomial design from the same authors
- [[SPAM-Tolerant Phase Estimation via Additive Error Bounds]]
