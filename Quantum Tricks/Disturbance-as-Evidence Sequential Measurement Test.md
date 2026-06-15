# Disturbance-as-Evidence Sequential Measurement Test

> **Source:** Harrow--Lin--Montanaro, arXiv:1607.03236, Appendix A
> **Tags:** #trick #sequential-measurements #disturbance #anti-Zeno #quantum-union-bound

## What it does

Turns measurement-induced disturbance into evidence that at least one measurement has substantial acceptance probability.

## The trick

Maintain a coherent branch containing the original state and a working branch exposed to random measurements. In each loop:

1. with small probability, compare the branches by a Hadamard-style disturbance test;
2. otherwise choose random $j$ and apply the measurement $\Lambda_j$ on the working branch.

If the working branch stays close to the original, the promised average acceptance probability gives a chance to accept. If the working branch drifts far away, the disturbance test catches it.

The analysis uses Gao's improved quantum union bound to control false positives in the case where every $\Lambda_j$ has small acceptance probability on the original state.

## When to reach for it

- Sequential-measurement settings where anti-Zeno drift is the obstacle.
- Cases where one can keep a coherent reference branch.
- Conceptual arguments where disturbance itself should count as success evidence.

## Complexity

For parameters $\eta,\zeta$, the protocol uses about

$$
\left\lceil \frac{5n}{\eta}+\frac{5}{\eta^2}\right\rceil
$$

iterations and separates acceptance $\ge\eta^2/7-O(1/n)$ from false acceptance $O(n\zeta/\eta)$.

## Caveat

The constants and parameter dependence are worse than the Marriott--Watrous average-measurement method. It is mainly a useful alternative proof pattern.

## Related notes

- [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]]
- [[Average-Measurement Marriott-Watrous OR Test]]
- [[Gentle Measurement Lemma]]
