# Confidence-Bound Greedy Allocation for Pauli Measurements

> **Source:** Huang, Kueng, Preskill, arXiv:2103.07510
> **Tags:** #trick #measurement-design #pauli-measurements #estimation

## What it does

Compresses a many-observable measurement-design problem into one scalar objective that can be optimized greedily.

## The trick

Instead of asking separately whether each target Pauli string has been measured enough, define

$$
\mathrm{Conf}_\varepsilon(O;P)=\sum_{\ell=1}^{L}\exp\!\left(-\frac{\varepsilon^2}{2}h(o_\ell;P)\right),
$$

where $h(o_\ell;P)$ is the number of measurement settings that hit observable $o_\ell$.

This objective has the right shape:

- each new hit on an under-measured observable buys a large multiplicative reduction;
- repeatedly hitting an already well-covered observable has diminishing returns;
- one measurement setting can help several observables at once.

Because the score factorizes nicely enough, you can update a conditional expected version of it after fixing one local basis choice at a time. That makes a combinatorial design problem tractable.

## When to reach for it

- You need to estimate many correlated observables simultaneously.
- Measurement settings can partially overlap with several targets.
- A naive per-term allocation misses shared structure.

## Complexity

The score can be updated locally as measurements are fixed. In the Pauli-setting application, it supports an $n\times M$ greedy pass over the measurement table.

## Caveat

A scalar objective is only as good as the failure model behind it. Here it is built from Hoeffding-style concentration plus a union bound, so it is conservative and may miss better globally coordinated strategies.

## Related notes

- [[Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021) — Paper Notes]]
- [[Derandomized Pauli Measurement Design via Conditional Expectations]]
- [[Median-of-Means Shadow Prediction]]
