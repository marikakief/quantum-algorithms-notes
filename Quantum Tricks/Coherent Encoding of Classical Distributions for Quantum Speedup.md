# Coherent Encoding of Classical Distributions for Quantum Speedup

> **Source:** Wocjan, Chiang, Abeyesinghe, Nagaj, arXiv:0811.0596  
> **Tags:** #trick #quantum-walk #state-preparation #partition-function #amplitude-estimation

## What it does

Converts a classical sampling problem into a quantum estimation problem by preparing a pure-state coherent encoding of a probability distribution, enabling phase estimation rather than repeated classical sampling.

## The trick

Given a classical distribution $\pi$ over a state space $\Omega$, prepare the quantum state

$$|\pi\rangle = \sum_{\sigma \in \Omega} \sqrt{\pi(\sigma)} |\sigma\rangle$$

instead of drawing independent samples from $\pi$. This "quantum sample" lets you estimate expectations $\mathbb{E}_\pi[f] = \sum_\sigma \pi(\sigma) f(\sigma)$ using [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]] with $O(1/\varepsilon)$ queries rather than the $O(1/\varepsilon^2)$ classical samples required by Chebyshev.

The preparation uses [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy quantum walks]]: given a reversible Markov chain $P$ with stationary distribution $\pi$ and spectral gap $\delta$, the quantum walk $W(P)$ has phase gap $\Theta(\sqrt{\delta})$, and $|\pi\rangle$ can be prepared in $O(1/\sqrt{\delta})$ walk steps.

For slowly-varying sequences of distributions $\pi_0, \pi_1, \ldots, \pi_\ell$ (where consecutive distributions have high overlap), prepare $|\pi_i\rangle$ from $|\pi_{i-1}\rangle$ via Grover's $\pi/3$-fixed-point search, driving the state through the sequence.

## When to reach for it

- Approximating partition functions via telescoping products of ratios
- Any problem where classical MCMC sampling is the bottleneck and you want a quadratic speedup in accuracy
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro-style]] quantum speedup of expected-value estimation
- Gibbs state preparation when you have access to a rapidly-mixing classical chain

## Complexity

$O(\ell/\sqrt{\delta} \cdot \text{polylog}(\ell))$ quantum walk invocations to prepare $|\pi_\ell\rangle$ from $|\pi_0\rangle$ through $\ell$ intermediate steps, each with spectral gap $\geq \delta$.

## Caveat

- Requires a classical Markov chain with a known lower bound on the spectral gap — without this, you can't bound the preparation cost.
- The encoding is a pure state, not a mixed state. Preparing $|\pi\rangle$ is strictly harder than sampling from $\pi$, but is necessary for the $1/\varepsilon$ speedup.
- If the distribution changes adversarially (not slowly-varying), the sequential preparation breaks down.

## Related notes

- [[Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Quantum Ratio Estimation via Phase Estimation]]
- [[Amplitude Estimation via Phase Estimation on Q]]
