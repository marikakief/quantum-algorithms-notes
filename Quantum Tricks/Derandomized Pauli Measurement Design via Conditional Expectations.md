# Derandomized Pauli Measurement Design via Conditional Expectations

> **Source:** Huang, Kueng, Preskill, arXiv:2103.07510
> **Tags:** #trick #measurement-design #classical-shadows #derandomization

## What it does

Turns a randomized Pauli-measurement protocol with a good average guarantee into a deterministic schedule that is guaranteed to be at least as good as the random average.

## The trick

Start from a randomized protocol where each single-qubit basis choice is sampled from $\{X,Y,Z\}$. Define a pessimistic estimator for failure — here the confidence bound

$$
\mathrm{Conf}_\varepsilon(O;P)=\sum_{\ell=1}^{L}\exp\!\left(-\frac{\varepsilon^2}{2}h(o_\ell;P)\right).
$$

Now reveal the $n\times M$ local basis choices one at a time. After some entries have been fixed, compute the conditional expectation of this failure bound over the remaining random entries:

$$
\mathbb E_P\big[\mathrm{Conf}_\varepsilon(O;P)\mid P^\sharp\big].
$$

At each step choose the basis $W\in\{X,Y,Z\}$ that minimizes the conditional expectation. By the method of conditional expectations, the final deterministic schedule $P^\sharp$ obeys

$$
\mathrm{Conf}_\varepsilon(O;P^\sharp)
\le
\mathbb E_P\big[\mathrm{Conf}_\varepsilon(O;P)\big].
$$

So you inherit the randomized guarantee without keeping any randomness.

## When to reach for it

- You have a good randomized design but want a deterministic schedule.
- The target observables are known in advance.
- The randomized scheme wastes too many shots on rare high-weight events.
- You can compute or cheaply approximate a conditional expected failure score.

## Complexity

For Pauli measurement design, each local choice compares three candidate assignments. The cost is classical preprocessing before experiments; the quantum measurement depth stays at single-qubit Pauli measurements.

## Caveat

This is a greedy derandomization, not a global optimizer. If the observable family has misleading local structure, the schedule can get trapped in a bad local pattern.

## Related notes

- [[Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021) — Paper Notes]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Confidence-Bound Greedy Allocation for Pauli Measurements]]
- [[Shadow Norm as Measurement-Observable Match]]
