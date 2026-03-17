# Integrated-Norm Equal-Segment Partitioning

> **Tags:** #trick #time-dependent #segmentation #error-budget
> **Source:** arXiv:1906.07115, Theorem 7'

## What it does

Splits the time interval $[0,t]$ into $r$ segments with equal **integrated norm**: choose boundaries $0 = t_0 < t_1 < \cdots < t_r = t$ such that

$$
\int_{t_{j-1}}^{t_j} \|H(\tau)\|_{\max}\,d\tau = \frac{\|H\|_{\max,1}}{r}
$$

for all $j$. Each segment then has the same "simulation budget," which allows the short-time error bound for continuous [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]] to be applied uniformly across segments without worst-case blowup from norm spikes.

## Why not uniform time segments

With uniform time slicing, a segment containing a large norm spike pays a much larger short-time error than others, forcing $r$ to be set by the worst-case norm rather than the average. Equal-integrated-norm segmentation ensures each segment has the same error and the global bound $4\|H\|_{\max,1}^2/r$ holds cleanly.

## Implementation

Requires computing the cumulative function $f(t) = \int_0^t \|H(\tau)\|_{\max}\,d\tau$ and finding the preimages $f^{-1}(j\|H\|_{\max,1}/r)$ for $j = 1, \ldots, r-1$. This is the same reparameterization oracle ($O_{\rm var}$, $O_{\rm norm}$) used in the rescaled-Dyson algorithm.

## Connection to time reparameterization

This segmentation is the discrete version of the continuous time reparameterization $s = f(t)$. The rescaled-Dyson algorithm applies this idea directly to the [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Dyson series]] (not just to the segment boundaries), achieving better asymptotic scaling (near-linear vs. quadratic in $\|H\|_{\max,1}$). Equal-segment partitioning is the simpler, weaker version used in the [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]] analysis.

## Related notes
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
- [[Continuous qDRIFT Time-Sampling by Instantaneous Norm]]
- [[Time-Reparameterization for Norm-Flattened Dyson Simulation]]
