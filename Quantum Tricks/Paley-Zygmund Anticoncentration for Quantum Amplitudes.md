# Paley-Zygmund Anticoncentration for Quantum Amplitudes

> **Source:** Bremner-Montanaro-Shepherd, arXiv:1504.07999
> **Tags:** #trick #anticoncentration #IQP #quantum-supremacy #probability

## What it does
Proves that the output probabilities of random quantum circuits are spread out (not concentrated near zero), which is needed to convert additive-error classical simulation to multiplicative-error estimation.

## The trick
For a family of random quantum circuits $C$, show that $|\langle 0|C|0\rangle|^2$ is not too concentrated near zero by bounding the fourth moment.

The **Paley-Zygmund inequality** states: for non-negative $R$ with finite variance,

$$\Pr[R \geq \alpha\, \mathbb{E}[R]] \geq (1 - \alpha)^2 \frac{\mathbb{E}[R]^2}{\mathbb{E}[R^2]}$$

Apply this to $R = |\langle 0|C|0\rangle|^2$:

1. **First moment** is easy: $\mathbb{E}[R] = 2^{-n}$ (by averaging over random bit flips and using normalisation).
2. **Second moment** (= fourth moment of amplitudes): compute $\mathbb{E}[|\langle 0|C|0\rangle|^4]$ by expanding into a sum over quadruples $(w, x, y, z) \in (\{0,1\}^n)^4$. Show that random phases kill all terms where $w, x, y$ are distinct — only the $3 \cdot 2^{2n}$ terms where two of $\{w, x, y\}$ coincide survive.
3. This gives $\mathbb{E}[R^2] \leq 3 \cdot \mathbb{E}[R]^2$, so:

$$\Pr\!\left[|\langle 0|C|0\rangle|^2 \geq \frac{1}{2} \cdot 2^{-n}\right] \geq \frac{1}{12}$$

A constant fraction of circuits have output probabilities within a constant factor of the uniform distribution — exactly what's needed for the Stockmeyer-based argument.

## When to reach for it
- Proving approximate hardness of sampling for a random circuit family (the "anticoncentration lemma" step)
- Any quantum supremacy argument that needs to go from additive to multiplicative error
- When you can compute or bound fourth moments of circuit amplitudes

## Complexity
The technique itself is just an inequality — zero computational cost. The work is in proving the fourth-moment bound, which depends on the algebraic structure of the circuit family.

## Caveat
The fourth-moment approach gives weak constants ($1/12$ fraction, $1/2$ threshold). For [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes|BosonSampling]], the analogous calculation involves the permanent of Gaussian matrices, where the fourth moment is hard to bound — this is why the permanent anticoncentration conjecture remains open.

## Related notes
- [[Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016) — Paper Notes]]
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]]
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]]
- [[Post-Selection Boosting for Hardness of Sampling]]
- [[X-Gate Random Self-Reduction for IQP]]
