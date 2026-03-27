# SPAM-Tolerant Phase Estimation via Additive Error Bounds

> **Source:** Kimmel, Low, Yoder, arXiv:1502.02677 (2015)
> **Tags:** #trick #phase-estimation #SPAM #Heisenberg-scaling #metrology

## What it does

Enables Heisenberg-limited phase estimation ($\sigma = O(1/N)$) even when state preparation and measurement are imperfect, by treating SPAM errors as bounded additive perturbations to measurement probabilities.

## The trick

In a standard phase estimation experiment, you prepare a state, apply $U^k$, and measure. The measurement probability ideally has the form:

$$p(A, k) = \frac{1 + \cos(kA)}{2}$$

SPAM errors shift this to:

$$p'(A, k) = \frac{1 + \cos(kA)}{2} + \delta(k)$$

The key observation: **SPAM errors contribute a constant additive shift $\delta$ that does not grow with $k$.** Gate errors and decoherence *do* grow with $k$, but SPAM is applied once at the start and once at the end — it doesn't accumulate.

To counteract additive errors of magnitude $\delta < 1/\sqrt{8} \approx 0.354$, increase the number of measurement samples at each step by a factor:

$$F(\delta, M) = \frac{\log\!\left(\frac{1}{2}(1 - \sqrt{8}\delta)^{1/M}\right)}{\log\!\left(1 - \frac{1}{2}(1 - \sqrt{8}\delta)^2\right)}$$

This factor is constant (independent of the total number of phase estimation rounds), so **Heisenberg scaling is preserved** — the total time increases by only a constant multiplicative overhead.

If the additive error grows with $k$ (e.g., depolarising noise with $\delta(k) = (1-\gamma^k)/2$), the protocol works up to the $k^*$ where $\delta(k^*) = 1/\sqrt{8}$, giving precision $O(1/k^*)$.

## When to reach for it

- Any [[Iterative Phase Estimation (Kitaev)|iterative phase estimation]] protocol where SPAM quality is uncertain.
- Gate calibration experiments where you can't assume perfect state preparation — you're calibrating the very gates you'd use to prepare states.
- Quantum metrology settings where the precision bottleneck is SPAM uncertainty, not shot noise.
- As a theoretical framework for arguing that SPAM errors don't destroy Heisenberg scaling in phase-based parameter estimation.

## Complexity

- **Overhead:** constant multiplicative factor $F$ in the total number of samples, depending on the additive error bound $\delta$.
- For $\delta = 0.1$: $F \approx 2$–$3$ (modest overhead).
- For $\delta \to 1/\sqrt{8}$: $F \to \infty$ (protocol degrades gracefully but loses Heisenberg scaling).

## Caveat

- Requires a known upper bound on $\delta$. If you overestimate $\delta$, you waste samples; if you underestimate, the protocol may fail.
- The $1/\sqrt{8}$ threshold is absolute. Beyond it, no amount of oversampling recovers the estimate.
- Only addresses *bounded* additive errors. Systematic biases that grow linearly with $k$ (e.g., coherent errors on top of the target unitary) need different treatment — they look like a shift in the phase being estimated, not an additive error on the probability.
- The protocol is non-adaptive. Adaptive approaches like [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes|Bayesian phase estimation]] can potentially handle SPAM more efficiently by incorporating noise models into the likelihood.

## Related notes
- [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Bayesian Restart Protocol for Phase Estimation]]
- [[Geometric Error Probability Bound for Non-Adaptive Phase Estimation]]
