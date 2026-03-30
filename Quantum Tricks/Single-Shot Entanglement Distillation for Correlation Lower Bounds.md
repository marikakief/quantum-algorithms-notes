# Single-Shot Entanglement Distillation for Correlation Lower Bounds

> **Source:** Brandão and Horodecki, arXiv:1206.2947
> **Tags:** #trick #entanglement-distillation #correlations #single-shot #smooth-entropy

## What it does
Converts an entropy imbalance in a tripartite pure state into a lower bound on the correlation function between two parties, by interpreting a random measurement on the third party as a single-shot entanglement distillation protocol.

## The trick
For a tripartite pure state $|\psi\rangle_{ABC}$, a random POVM on $A$ with $N \approx 2^{I_{\max}^\delta(A:B)}$ outcomes can produce a post-measurement state that is approximately a maximally entangled state between (a subspace of) $A$ and $C$. The entanglement distillation rate is governed by the smooth conditional min-entropy $H_{\min}^\delta(A|B)$: when this is positive, EPR pairs can be distilled.

This gives a lower bound on correlations: if $H_{\min}^\delta(A|B) \geq -2\log(\delta) + 5$, then

$$\mathrm{Cor}(A:C) \geq \left(\frac{1}{256} - 26\sqrt{\delta}\right) 2^{-(I_{\max}^\delta(A:B) - 2\log(\delta) + 5)}$$

Even when $H_{\min}^\delta(A|B) \leq 0$ (no EPR pairs can be distilled), there is a second bound: if the max-entropy of $C$ is much larger than $B$'s, $H_{\max}^\nu(C) \geq 3 H_{\max}^\delta(B)$, then

$$\mathrm{Cor}(A:C) \geq (\text{const}) \cdot 2^{-3(H_{\max}^\delta(B) + O(\log 1/\delta))}$$

This second regime is novel — it shows that a random measurement on $A$ generates correlations with $C$ even without distilling entanglement, as long as $C$ has enough entropy.

## When to reach for it
- Proving area laws from correlation bounds (the primary application)
- Any argument where you need to show that small correlations between separated regions force entropy constraints
- Relating single-shot entropies to operationally measurable correlation functions

## Complexity
Not a computational technique — a proof tool. The "measurement" has $2^{I_{\max}(A:B)}$ outcomes, so the classical communication cost of the distillation protocol is the max-mutual information.

## Caveat
Requires a pure tripartite state (purity is used to relate $H(AC)$ to $H(B)$). The mixed-state case requires additional terms involving $H_{\max}(\rho)$. The single-shot entropy quantities (smooth min/max) are harder to work with than von Neumann entropies, and the proof involves multiple layers of smoothing parameters that must be tracked carefully.

## Related notes
- [[Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) — Paper Notes]]
- [[Negative Conditional Entropy as Entanglement Gain]]
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]]
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]]
