# Adaptive Spectral-Amplified Energy Estimation

> **Source:** King, Low, Babbush, Somma, Rubin, arXiv:2505.01528 (Theorems 5 & 7)
> **Tags:** #trick #energy-estimation #phase-estimation #spectral-amplification #adaptive

## What it does

Estimates the expectation $E = \langle\psi|H|\psi\rangle$ or the ground-state energy of a [[Block-Encoding Composition Algebra|block-encoded]] Hamiltonian with query complexity $O(\sqrt{\max\{\varepsilon, \Delta\}\cdot\lambda}/\varepsilon)$, **without** requiring prior knowledge of the low-energy parameter $\Delta$.

## The trick

The standard approach with [[SOS Spectral Amplification|spectral amplification]] needs an a priori upper bound $\Delta$ on the energy gap to set the phase estimation precision. Without it, you'd default to worst-case $\varepsilon' = \varepsilon/\lambda$, losing the spectral amplification advantage.

The adaptive algorithm avoids this in two steps:

**Step 1 (Coarse):** Run [[Gapped Phase Estimation]] on the [[Qubitization Iterate|walk operator]] at accuracy $\varepsilon' = \Theta(\sqrt{\varepsilon/\lambda})$. Two outcomes:
- $\lambda + E \leq \varepsilon$ → terminate, return $-\lambda$ (correct to error $\varepsilon$)
- $\lambda + E > \varepsilon$ → obtain a *constant-factor multiplicative* estimate $\hat{E}$ of $\lambda + E$

**Step 2 (Fine):** Using $\hat{E}$, we now know the number of binary search iterations $i_{\max} = O(\log_{3/2}(\sqrt{\hat{E}\lambda}/\varepsilon))$ to within an additive constant. Run $i_{\max}$ rounds of [[Gapped Phase Estimation]], each splitting the current interval into thirds and deciding which third contains the eigenphase. Choose per-round failure probabilities $q_i = \frac{6}{\pi^2} \cdot \frac{q}{(i_{\max} - i + 1)^2}$ so the union bound over all rounds gives total failure probability $\leq q$.

The first step costs $O(\sqrt{\lambda/\varepsilon}\log(1/q))$ queries. The second costs $O(\sqrt{\hat{E}\lambda}/\varepsilon \cdot \log(1/q))$. Total: $O(\sqrt{\max\{\varepsilon, \lambda + E\}\cdot\lambda}/\varepsilon \cdot \log(1/q))$.

For phase estimation with overlap $p$, each binary search step wraps the [[Gapped Phase Estimation]] in an amplitude estimation call (estimating the probability of the GPE outcome), costing $O(1/\sqrt{p})$ per round.

## When to reach for it

- Any low-energy simulation task where the energy gap $\Delta$ isn't known in advance
- Ground-state energy estimation with imperfect trial states ($p < 1$)
- Settings where the "super-Heisenberg" regime $\varepsilon \sim \Delta$ gives $O(\sqrt{\lambda/\varepsilon})$ scaling — the algorithm automatically achieves this without needing to know you're in this regime

## Complexity

**Energy estimation:** $O\!\left(\frac{\sqrt{\max\{\varepsilon, \lambda - |E|\}\cdot\lambda}}{\varepsilon}\log\frac{1}{q}\right)$ queries, 2 ancillary qubits.

**Phase estimation:** $O\!\left(\frac{\sqrt{\max\{\varepsilon, \lambda + E\}\cdot\lambda}}{\sqrt{p}\,\varepsilon}\log\frac{1}{p}\log\frac{1}{q}\right)$ queries to block-encoding; $O\!\left(\frac{1}{\sqrt{p}}\log\frac{\lambda}{\varepsilon}\log\frac{\log(\lambda/\varepsilon)}{q}\right)$ queries to state preparation.

## Caveat

The union-bound engineering is tight — the $\log(1/q)$ confidence scaling (rather than $\log\log(\lambda/\varepsilon)/q$) depends on knowing $i_{\max}$ from the coarse step. If the coarse estimate is wrong (failure probability $\leq q/2$), the whole algorithm fails. Also, this is a query complexity result — the per-query gate cost still depends on the block-encoding construction.

## Related notes

- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes]] — source paper
- [[SOS Spectral Amplification]] — the amplification framework these algorithms build on
- [[Gapped Phase Estimation]] — the binary search subroutine
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — chemistry application
- [[SOSSA — Sum-of-Squares Spectral Amplification]] — informal paper note
