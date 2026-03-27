
> **Source:** Cleve, Gottesman, Mosca, Somma, Yonge-Mallo (CGMSY 2009); Berry, Cleve, Gharibian, arXiv:1211.4637
> **Tags:** #trick #error-correction #probabilistic #simulation

## What it does

Converts a simulation step that succeeds with probability $\geq 3/4$ into one that succeeds with probability $\geq 1 - \varepsilon$ using $O(1)$ expected repetitions, without increasing the asymptotic gate cost.

## The trick

Suppose a quantum simulation step produces the correct output conditioned on a measurement outcome that occurs with probability $p \geq 3/4$. On failure, the errors are *unitary* — the step can be undone.

Model the correction as a biased random walk on $\mathbb{Z}$:
- **Success** (probability $\geq 3/4$): step right
- **Failure** (probability $\leq 1/4$): step left (undo and retry)

Start at position 0. The simulation succeeds when the walk reaches position +1. For bias $p \geq 3/4$, the expected hitting time is $O(1)$ (specifically, $1/(2p-1) \leq 2$).

The undo step: invert the construction from Figure 2 in CGMSY, applying each error correction in reverse. The inversion itself succeeds with probability $\geq 3/4$, so failure to undo triggers another undo attempt — the walk continues in both directions.

**Bounding the tail:** By Markov's inequality, the probability that the walk takes more than $O(1/\varepsilon)$ steps is $O(\varepsilon)$. This failure probability is absorbed into the overall error budget.

## When to reach for it

- You have a simulation step that's correct conditioned on a "good" measurement outcome
- Failed outcomes are detectable and reversible (the error is a known unitary)
- You need $O(1)$ expected overhead, not $O(\log(1/\varepsilon))$
- You can tolerate absorbing rare worst-case events into the total error

This is common in ancilla-based simulation schemes where a control qubit is measured and the "wrong" outcome indicates a correctable error.

## Complexity

- Expected repetitions: $O(1)$ for bias $\geq 3/4$
- Worst-case cap at $O(1/\varepsilon)$ repetitions with failure probability $O(\varepsilon)$
- Each repetition costs the same as one simulation step (plus its inverse)

## Caveat

The overhead is $O(1)$ only in expectation. For algorithms requiring deterministic gate counts (e.g., for circuit compilation), the tail bound matters. Also, the errors must be exactly unitary and exactly invertible — approximate errors require a separate analysis.

The constant factor hidden in the expected repetitions can be meaningful for resource estimation, even though it doesn't affect asymptotics.

## Related notes

- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]] — uses this technique for CGMSY segment correction
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes]] — also uses biased random walk correction
- [[Standard Amplitude Amplification]] — a different approach to boosting success probability (fixed overhead, but requires additional ancillae)
