# Bounded-Variance Streaming Estimators via Quantum Mean Estimation

> **Source:** Montanaro, arXiv:1505.00113; Montanaro, arXiv:1504.06987
> **Tags:** #trick #streaming #mean-estimation #frequency-moments #amplitude-estimation

## What it does

Turns a classical low-variance streaming estimator into a quantum multi-pass algorithm with near-quadratic improvement in precision scaling.

## The trick

Given a randomised estimator $v(A)$ with bounded relative second moment

$$
\frac{\mathbb E[v(A)^2]}{\mathbb E[v(A)]^2}\le B,
$$

apply [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|quantum mean estimation]] to estimate $\mathbb E[v(A)]$ to relative error $\epsilon$ using

$$
O\!\left(\frac{B}{\epsilon}\log^{3/2}(B/\epsilon)\log\log(B/\epsilon)\right)
$$

coherent calls to the estimator and inverse.

For $F_2$, use the AMS estimator

$$
f(h)=\left(\sum_i h(a_i)\right)^2,
$$

where $h:[m]\to\{\pm1\}$ is 4-wise independent, so

$$
\mathbb E[f]=F_2,\qquad \operatorname{Var}(f)\le2F_2^2.
$$

## When to reach for it

- Classical streaming estimators with bounded relative variance.
- Settings where one can compute and uncompute the estimator coherently with a small number of passes.
- Replacing $O(1/\epsilon^2)$ classical averaging by roughly $\widetilde O(1/\epsilon)$ quantum averaging.

## Complexity

For streaming $F_2$, space is

$$
O(\log n+\log(1/\epsilon))
$$

and pass complexity is

$$
O\!\left(\frac{1}{\epsilon}\log^{3/2}(1/\epsilon)\log\log(1/\epsilon)\right).
$$

## Caveat

The estimator must be coherently reversible and have a worst-case bounded implementation cost. Expected-runtime estimators do not plug in automatically.

## Related notes

- [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
