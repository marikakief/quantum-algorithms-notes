# Typical Subspace Projection for One-Shot Bounds

> **Source:** Horodecki, Oppenheim & Winter, arXiv:quant-ph/0512247; used throughout quantum Shannon theory
> **Tags:** #trick #typical-subspace #one-shot #quantum-shannon-theory #AEP

## What it does

Converts a one-shot quantum information protocol — stated in terms of Hilbert space dimensions and purities — into an asymptotic protocol with rates given by von Neumann entropies, by projecting onto typical subspaces.

## The trick

For $n$ copies of a state $\rho_A$ with eigenvalues $\{\lambda_i\}$, the typical subspace $\tilde{A}$ (with tolerance $\delta$) contains all sequences whose empirical distribution is $\delta$-close to $\{\lambda_i\}$. Key properties:

1. **High overlap:** $\operatorname{Tr}(\rho_A^{\otimes n} \Pi_{\tilde{A}}) \geq 1 - e^{-c\delta^2 n}$
2. **Dimension control:** $2^{n[S(A) - \delta]} \leq \dim(\tilde{A}) \leq 2^{n[S(A) + \delta]}$
3. **Eigenvalue control:** $\Pi_{\tilde{A}} \rho_A^{\otimes n} \Pi_{\tilde{A}} \leq 2^{-n[S(A) - \delta]} \Pi_{\tilde{A}}$

To use: start with a one-shot result that depends on dimensions $d_{\tilde{A}}$, $d_{\tilde{R}}$ and effective dimension $D$ (via $\operatorname{Tr}(\rho_B^2) \leq 1/D$). Project onto typical subspaces, use the gentle measurement lemma to bound the error introduced by projection, and substitute:

$$d_{\tilde{A}} \approx 2^{nS(A)}, \quad d_{\tilde{R}} \approx 2^{nS(R)}, \quad D \approx 2^{nS(B)}$$

The one-shot error bounds then decay exponentially in $n$, and the rates converge to entropic quantities.

## When to reach for it

- Any time you have a one-shot quantum information result involving dimensions and purities and want to extract asymptotic rates.
- Proving achievability in quantum Shannon theory: source coding, state merging, channel coding, entanglement distillation.
- Converting purity-based bounds ($\operatorname{Tr}(\rho^2)$) to entropy-based bounds ($S(\rho)$).

## Complexity

The projection itself is efficient if you can implement the typical subspace projector, which requires knowledge of the eigenvalues of $\rho$ (known by assumption in Shannon theory). The cost is dominated by whatever protocol uses the projected state.

## Caveat

Requires the i.i.d. assumption ($\rho^{\otimes n}$). For non-i.i.d. or adversarial settings, you need smooth entropy methods instead. The gentle measurement lemma introduces an additive error of $O(e^{-c\delta^2 n/2})$ per projection, which is negligible asymptotically but matters for finite $n$.

## Related notes
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]] — primary application
- [[Decoupling by Random Measurement]]
- [[Gentle Measurement Lemma]]
