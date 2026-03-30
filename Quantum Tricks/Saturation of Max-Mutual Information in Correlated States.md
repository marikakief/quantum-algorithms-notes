# Saturation of Max-Mutual Information in Correlated States

> **Source:** Brandão and Horodecki, arXiv:1206.2947 (building on Hastings, arXiv:0705.2024)
> **Tags:** #trick #mutual-information #smooth-entropy #1D-systems #area-law

## What it does
Guarantees the existence of a region in a 1D chain where the smooth max-mutual information between the region's interior and its boundary is small. This is the single-shot upgrade of a von Neumann entropy saturation result used by [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes|Hastings]] in his area law proof.

## The trick
For a state $\rho_{1,\ldots,n}$ on a line with $(\xi, l_0)$-exponential decay of correlations and any target site $s$: for all $\delta > 0$ and $\min(\delta/2, 1/\xi) \geq \epsilon > 0$, there exists a connected region $X_{2l} = X_{L,l/2} X_{C,l} X_{R,l/2}$ with $O(\xi^2 \log(2/\epsilon)/\epsilon) \leq l/l_0 \leq \exp(O(\log(1/\epsilon)/\epsilon))$, centred at most $l_0 \exp(O(\log(1/\epsilon)/\epsilon))$ sites from $s$, such that:

$$I_{\max}^\delta(X_{C,l} : X_{L,l/2} X_{R,l/2}) \leq \frac{8\epsilon l}{\delta} + \delta$$

The von Neumann version (Hastings' original, Lemma 16 in the paper) follows from repeated subadditivity: if mutual information doesn't eventually saturate, entropy would have to grow faster than the volume bound allows. The single-shot version is harder because max-entropy and min-entropy can differ significantly, so simple subadditivity chains don't work.

The resolution uses two ideas:
1. **Quantum substate theorem** (Jain-Radhakrishnan-Sen): converts max-mutual information to a hybrid quantity involving both von Neumann and max entropies
2. **Quantum equipartition property** + exponential decay of correlations: upper-bounds max-entropy by a sum of von Neumann entropies across nearby sites

## When to reach for it
- Proving single-shot area laws from correlation decay
- Any argument requiring control over single-shot entropy quantities in 1D correlated states
- Finding "nice" regions in a chain where entropies are well-behaved (useful for tensor network constructions)

## Complexity
Not a computational technique. The region can be found at distance at most $l_0 \exp(O(\log(1/\epsilon)/\epsilon))$ from any desired site, so the search window is exponential in $1/\epsilon$ (which is set to $\sim \xi$).

## Caveat
The exponential search window ($l_0 \exp(O(\xi \log \xi))$) is what makes the final area law bound exponential in $\xi$. If this could be improved to polynomial, the area law bound would improve correspondingly. Also, the proof only works in 1D — the subadditivity chain relies on the linear geometry of the chain.

## Related notes
- [[Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) — Paper Notes]]
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]]
- [[Mutual Information Gap via Relative Entropy Monotonicity]]
- [[Single-Shot Entanglement Distillation for Correlation Lower Bounds]]
