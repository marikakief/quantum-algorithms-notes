# Permutation-Observable Noise Deconvolution for Multi-Copy Moments

> **Source:** Kannan, Putterman, and Cotler, arXiv:2605.02057
> **Tags:** #trick #multi-copy #moments #noise #observable-estimation

## What it does

Estimates nonlinear state moments from noisy copies by applying the inverse noise channel to the permutation observable rather than trying to physically undo the noise on the state. This is calibrated classical post-processing: it removes bias only at the cost of potentially large variance.

## The trick

For three copies, the cyclic permutation $C=\pi_{123}$ satisfies

$$
\operatorname{tr}(C\rho^{\otimes 3})=\operatorname{tr}(\rho^3).
$$

Since $C$ is not Hermitian, use

$$
H=\frac{C+C^{-1}}{2},
\qquad
\operatorname{tr}(H\rho^{\otimes 3})=\operatorname{tr}(\rho^3).
$$

Now suppose each copy has passed through local depolarising noise $D_{\lambda'}^{\otimes n}$. Let $I_{\lambda'}$ be the inverse channel on Pauli operators:

$$
I_{\lambda'}[P]=
\begin{cases}
(1-\lambda')^{-1}P, & P\ne I,\\
P, & P=I.
\end{cases}
$$

Instead of trying to invert the noisy state, measure the deconvolved observable

$$
H_{\lambda'}=(I_{\lambda'}^{\otimes n})^{\otimes 3}[H]
$$

on the noisy three-copy state. Then

$$
\operatorname{tr}\!\left(H_{\lambda'}(D_{\lambda'}^{\otimes n}[\rho])^{\otimes 3}\right)
=\operatorname{tr}(\rho^3).
$$

Operationally, the paper diagonalises the local three-cycle on the three copies at each physical site, using the eigenbasis of the one-site cyclic permutation. If outcomes are $s_1,\ldots,s_n\in\{1,\omega,\omega^2\}$, the single-shot estimator is

$$
X=\operatorname{Re}\!\left(\prod_{j=1}^n \alpha_{s_j}\right),
$$

where the $\alpha_s$ are the eigenvalues of the one-site corrected operator.

## When to reach for it

- Estimating $\operatorname{tr}(\rho^t)$ or other permutation-polynomial moments from noisy copies.
- Error-mitigation-style protocols where the observable is known exactly and noise is a calibrated Pauli-diagonal channel.
- Comparing raw multi-copy processing with [[Quantum Uploading as One-Time Input Noise]].

## Complexity

The estimator is unbiased, but the range grows with inverse powers of $(1-\lambda')$. For the cubic moment, the 2026 version of the paper obtains a theorem-specific sample complexity of the form

$$
O\!\left(
\frac{1}{\epsilon^2}
\left(\frac{1-6a^{-2}+9a^{-4}+12a^{-6}}{16}\right)^n
\log\frac1\delta
\right),
\qquad a=1-\lambda'.
$$

At zero noise this is constant-sample for the hard promise problem; as $\lambda'\to 1$, the inverse channel blows up.

## Caveat

This is deconvolution, not magic. It trades bias for variance. If the uploaded input noise is too high, the inverse-noise factors dominate and the advantage disappears.

It also requires a reliable noise model. Miscalibrated deconvolution can be worse than accepting the noise bias.

It is not physical error correction: no operation denoises the state. All correction is done by changing the measured observable / estimator.

## Related notes

- [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]]
- [[Swap Test for State Comparison]]
- [[Density Matrix Exponentiation via Partial Swap]]
- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes]]
- [[Quantum Uploading as One-Time Input Noise]]
