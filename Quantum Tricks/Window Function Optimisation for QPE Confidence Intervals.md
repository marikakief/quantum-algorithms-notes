# Window Function Optimisation for QPE Confidence Intervals

> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748 (2024)
> **Tags:** #trick #phase-estimation #window-functions #Kaiser #fault-tolerant #signal-processing

## What it does

Optimises the control register state in quantum phase estimation to minimise the width of a confidence interval at a given cost (number of controlled walk operator applications). Uses window functions from signal processing theory — specifically the Kaiser window and the Slepian prolate spheroidal window — instead of the standard uniform or cosine windows.

## The trick

In QPE, the control register is initialised in $|\Gamma\rangle = \sum_n \gamma_n |n\rangle$. The standard textbook approach uses $\gamma_n = 1/\sqrt{2N}$ (uniform superposition), giving slow $O(1/k^2)$ tail decay in the error distribution. The cosine window minimises variance but doesn't optimise tail probabilities.

For confidence intervals, the goal is different: minimise the probability $\delta$ that the phase error exceeds a threshold $\varepsilon_\phi$, given $N$ controlled walk applications.

**Kaiser window:** Set $\gamma_n \propto I_0(\pi\alpha\sqrt{1 - ((n - N + 1/2)/N)^2})$ where $I_0$ is the modified Bessel function. The error distribution becomes:

$$|\Gamma(\theta)|^2 \propto \frac{\sin^2\sqrt{N^2\theta^2 - \pi^2\alpha^2}}{N^2\theta^2 - \pi^2\alpha^2}$$

The mainlobe has width $(\pi/N)\sqrt{1 + \alpha^2}$, and tails decay exponentially: $\delta \approx 4\ln(2\alpha)\sqrt{\alpha}\, e^{-2\pi\alpha}$.

The cost (number of controlled walk applications) to achieve confidence level $1 - \delta$ with error $\leq \varepsilon$ is:

$$N = \frac{\ln(1/\delta)}{2\varepsilon} + \frac{\ln(8\ln(4/\delta)/\pi)}{4\varepsilon} + O\left(\frac{\ln\ln(1/\delta)}{\varepsilon}\right)$$

**Prolate spheroidal window (optimal):** Uses the angular spheroidal function $\text{PS}_{0,0}(c, z)$. Gives the tightest possible confidence interval for given $N$, but the cost formula differs only in removing a triple-logarithmic correction term. The practical difference from the Kaiser window is $< 5\%$ and often $< 1\%$.

**Key insight for sampling with excited states:** When using QPE to estimate the ground state energy via repeated sampling and taking the minimum, excited states contribute to errors asymmetrically. The Kaiser window actually outperforms the optimal prolate spheroidal window in this multi-sample setting, because its stronger tail suppression reduces the probability of excited-state measurements producing catastrophically low energy estimates. The prolate spheroidal window concentrates spectral weight more efficiently in the mainlobe but has slightly fatter far tails.

**Adjustable cutoff:** Replacing the mainlobe width from $\pi\sqrt{1 + \alpha^2}$ to $\pi\sqrt{\Delta^2 + \alpha^2}$ for $\Delta < 1$ further improves performance by trading mainlobe width for tail suppression.

## When to reach for it

- Any fault-tolerant QPE-based algorithm where the cost per controlled walk application is high (thousands of Toffolis) and you want to minimise the total number of applications for a given confidence level.
- Ground state energy estimation with imperfect initial state overlap — the sampling and binary search methods in the source paper both use this.
- [[Kaiser Window Amplitude Estimation|Amplitude estimation]] subroutines within binary search or other iterative algorithms.
- Whenever you see a QPE analysis that uses the "textbook" rectangular window — switching to Kaiser typically reduces cost by a constant factor that's significant at practical parameters.

## Complexity

$N = \frac{\ln(1/\delta)}{2\varepsilon} + O(\varepsilon^{-1}\ln\ln(1/\delta))$ for confidence level $1 - \delta$ and error $\varepsilon$. Compare to $N = \pi/(2\varepsilon)$ for minimum variance (cosine window) — the confidence interval cost replaces $\pi$ with $\ln(1/\delta)$.

## Caveat

- The window function only controls the phase estimation error ("spectral leakage"). It doesn't address the problem of distinguishing which eigenstate was measured — that requires the sampling or binary search framework on top.
- The Slepian prolate spheroidal window is optimal but hard to compute. The Kaiser window is within a few percent and uses standard Bessel functions.
- The corrected higher-order terms for the Slepian window (fixing Slepian's 1965 error) are $-7/(16c)$ and $-91/(2^9 c^2)$, not $-3/(32c)$ as Slepian originally published.

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[Kaiser Window Amplitude Estimation]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Dolph-Chebyshev Eigenstate Filtering]]
- [[Iterative Phase Estimation (Kitaev)]]
