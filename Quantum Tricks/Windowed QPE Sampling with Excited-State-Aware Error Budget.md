# Windowed QPE Sampling with Excited-State-Aware Error Budget

> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748
> **Tags:** #trick #phase-estimation #window-functions #ground-state #fault-tolerant #sampling

## What it does

Turns repeated phase estimation plus “take the minimum energy” into a controlled confidence-interval procedure that does not get wrecked by low outliers.

## The trick

If one repeats [[Iterative Phase Estimation (Kitaev)|QPE]] $n$ times and returns the minimum estimated energy, the dangerous errors are asymmetric.

- A sample that is too **high** often just means one hit an excited state.
- A sample that is too **low** can fake a ground-state estimate.

So do not assign one symmetric union bound to everything. Instead, choose a windowed phase-estimation protocol, typically using a [[Kaiser Window Amplitude Estimation|Kaiser window]], with per-shot tail probability $\delta$, and budget the two directions separately.

The coarse bound is

$$[1-p(1-\delta/2)]^n + 1-(1-\delta/2)^n = q,$$

where $p$ is the initial squared ground-state overlap and $q$ is the final failure probability.

A sharper version inserts the excited-state location through probabilities $\delta_1$ and $\delta_2$ for an excited-state sample being too high or too low relative to the desired ground-state interval:

$$P_{\mathrm{err}} = [p\delta/2 + (1-p)\delta_1]^n + 1 - \left(1 - [p\delta/2 + (1-p)\delta_2]\right)^n.$$

This matters because a nearby excited state is not purely bad news. It can also increase the chance that a sample lands in the acceptable interval.

## When to reach for it

- Ground-state energy estimation where the input state already has moderate or high overlap.
- Fault-tolerant chemistry where a few extra QPE repetitions are cheaper than a more elaborate search routine.
- Any repeated-estimation setting where the returned statistic is an extremum rather than a mean.

## Complexity

With windowed QPE the query cost is roughly

$$
\frac{\lambda \ln(2/q)}{2p\epsilon}
\ln\!\left(\frac{\ln(2/q)}{pq}\right)
$$

at leading order for confidence interval half-width $\epsilon$.

## Caveat

This helps when $p$ is not too small. If $p$ drops below about $10^{-3}$ to $10^{-2}$, depending on constants, binary-search methods with amplitude estimation can overtake it.

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[Kaiser Window Amplitude Estimation]]
- [[Iterative Phase Estimation (Kitaev)]]
