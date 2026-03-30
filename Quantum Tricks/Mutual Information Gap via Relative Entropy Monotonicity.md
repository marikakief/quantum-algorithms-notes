# Mutual Information Gap via Relative Entropy Monotonicity

> **Source:** Hastings, arXiv:0705.2024
> **Tags:** #trick #entropy #mutual-information #relative-entropy #area-law

## What it does
Converts a gap between the expectation values of a local operator on the true state versus a product state into a lower bound on the mutual information (and hence the entanglement) between the two halves of a bipartition.

## The trick
The Lindblad-Uhlmann theorem states that relative entropy is monotone under completely positive trace-preserving maps. Applied to the relative entropy $S(\rho_{AB} \| \rho_A \otimes \rho_B)$ = mutual information $I(A:B)$, and using a measurement channel that extracts the expectation value of a positive operator $O_B$ with $\|O_B\| \leq 1$:

$$S(\rho_A) + S(\rho_B) - S(\rho_{AB}) \geq (1 - 2\epsilon) \ln\!\left(\frac{1-2\epsilon}{x}\right) + 2\epsilon \ln\!\left(\frac{2\epsilon}{1-x}\right)$$

where $1 - 2\epsilon = \operatorname{tr}(\rho_{AB} O_B)$ (the operator has high expectation on the true state) and $x = \operatorname{tr}((\rho_A \otimes \rho_B) O_B)$ (low expectation on the product state). The parameter $\epsilon$ comes from the accuracy of the [[Approximate Ground State Projector from Local Operators|local projector decomposition]].

When $\epsilon \to 0$ and $x \to 0$, this gives $I(A:B) \gtrsim \ln(1/x)$. The operator $O_B$ acts as a "witness" for entanglement across the cut: it can distinguish the entangled ground state from the product of its marginals.

## When to reach for it
- Proving area laws: need to show that entropy *cannot* grow too fast, by showing a corrected subadditivity inequality
- Any situation where you have an operator that distinguishes an entangled state from a product state, and you want to quantify the entanglement
- Lower-bounding mutual information from operator expectation value gaps

## Complexity
No computational cost — this is a proof technique, not an algorithm. The strength of the bound depends on the gap between the two expectation values.

## Caveat
The operator $O_B$ must be supported on the boundary region and have norm $\leq 1$. Constructing such an operator requires the spectral gap (via the local projector decomposition). The bound becomes trivial when $x$ is close to $1-2\epsilon$, i.e., when the operator can't distinguish the true state from the product state.

## Related notes
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]]
- [[Approximate Ground State Projector from Local Operators]]
- [[Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) — Paper Notes]]
