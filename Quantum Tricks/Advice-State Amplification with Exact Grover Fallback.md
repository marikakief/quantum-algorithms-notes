# Advice-State Amplification with Exact Grover Fallback

> **Source:** Montanaro, arXiv:0908.3066
> **Tags:** #trick #amplitude-amplification #quantum-search #state-preparation #Las-Vegas

## What it does

Uses a coherent advice distribution $|\mu\rangle$ to find a marked item faster on likely inputs, while an exact Grover fallback keeps the algorithm Las Vegas.

## The trick

Suppose a state-preparation oracle gives

$$
O_\mu |0\rangle = |\mu\rangle = \sum_x \sqrt{p_x}|x\rangle.
$$

If $x$ is the unique marked item, then starting [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] from $|\mu\rangle$ has initial success probability $p_x$. After $k$ iterations, the success probability is

$$
\sin^2\!\left((2k+1)\arcsin\sqrt{p_x}\right).
$$

When $p_x$ is unknown, choose the iteration count randomly from a geometrically growing range, as in [[Randomised Iteration Count for Grover Search]]. This gives constant success probability once the range reaches order $1/\sqrt{p_x}$.

Then add a final exact Grover search over the whole domain. That final step is not for speed; it converts the bounded-success schedule into a zero-error algorithm with finite expected cost on every input.

## When to reach for it

- Search with a coherent prior distribution over candidate solutions.
- Algorithms where the success amplitude is input-dependent and unknown.
- Settings where bounded-error search is not enough and a Las Vegas guarantee is needed.

## Complexity

For item $x$, Montanaro's version costs at most

$$
\min\{83/\sqrt{p_x}+4/3,\;53\sqrt n\}
$$

queries to each of $f$, $O_\mu$, and $O_\mu^{-1}$.

## Caveat

The oracle $O_\mu$ is a strong assumption. Classical sampling from $\mu$ is not enough; the trick needs coherent preparation and reflection about $|\mu\rangle$.

## Related notes

- [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]]
- [[Randomised Iteration Count for Grover Search]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]]
