# Geometric Error Probability Bound for Non-Adaptive Phase Estimation

> **Source:** Kimmel, Low, Yoder, arXiv:1502.02677 (2015); improves Higgins et al. (2009)
> **Tags:** #trick #phase-estimation #probability-bound #Heisenberg-scaling #analysis

## What it does

Provides an exponentially tight bound on the per-round error probability in non-adaptive phase estimation, improving the analytic scaling constant from $\sigma(\hat{A})T < 54\pi$ (Higgins et al.) to $\sigma(\hat{A})T < 10.7\pi$.

## The trick

In Higgins-style non-adaptive phase estimation, you estimate phase $A$ using experiments at $k_j = 2^{j-1}$ for $j = 1, \ldots, K$. Each round $j$ uses $M_j$ samples of both cosine and sine experiments, producing an estimate $\hat{A}_j$. An error at round $j$ occurs when $|k_j(\hat{A}_j - A)| \geq \pi/2$.

**Higgins et al.** bounded this error probability using Hoeffding's inequality, giving a loose bound that required large $M_j$ (setting $\alpha = 8\ln 2$).

**The geometric argument:** Represent the estimate as a point $(\hat{a}_0, \hat{a}_+)$ in the plane, where $\hat{a}_0$ and $\hat{a}_+$ are the number of successful outcomes of the cosine and sine experiments. The phase estimate $\hat{\phi} = \text{atan2}(\hat{a}_+ - M/2,\; \hat{a}_0 - M/2)$.

Key observations:
1. The error probability is maximised at $\phi = \pi/4$ (verified by analysing the mean and variance of the inner product $\hat{r} = (\hat{x}, \hat{y})\cdot(\cos\phi, \sin\phi)$).
2. At $\phi = \pi/4$, the two experiments have equal probabilities $p_0 = p_+ = (2+\sqrt{2})/4$.
3. Errors correspond to outcomes along diagonal lines $\hat{a}_0 + \hat{a}_+ = b$ for $b \leq M$.
4. Summing these using the Vandermonde identity and Stirling's approximation:

$$p_{\text{error}}(\phi) < \frac{1}{\sqrt{2\pi M}\,2^M}$$

This is asymptotically tight (exact in the $M \to \infty$ limit) and exponentially smaller than the Hoeffding bound for moderate $M$.

**Result:** setting $M_j = \frac{5}{2}(K-j) + \frac{1}{2}$ gives $\sigma(\hat{A})T < 10.7\pi$, a ~5× improvement in the scaling constant.

## When to reach for it

- Analysing non-adaptive phase estimation protocols based on the Higgins framework.
- Any setting where you need tight bounds on the error probability of an atan2 estimator from binomial samples.
- As a template for geometric analysis of estimation errors in 2D (cosine + sine) measurement schemes.

## Complexity

No computational overhead — this is an analysis technique that reduces the required $M_j$ at each step, saving total experimental time by a constant factor (~5×).

## Caveat

- Specific to the 2-experiment (cosine + sine) structure of [[Iterative Phase Estimation (Kitaev)|iterative phase estimation]]. Doesn't directly apply to single-experiment schemes.
- The bound is for the worst-case $\phi = \pi/4$. For specific known ranges of $\phi$, tighter bounds exist.
- The $10.7\pi$ constant is for the non-adaptive case. Adaptive schemes like [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes|Bayesian phase estimation]] can achieve better constants by optimising experiment design.

## Related notes
- [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[SPAM-Tolerant Phase Estimation via Additive Error Bounds]]
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]]
