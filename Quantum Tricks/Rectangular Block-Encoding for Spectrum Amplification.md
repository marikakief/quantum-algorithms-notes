# Rectangular Block-Encoding for Spectrum Amplification

> **Source:** Low, King, Berry et al., arXiv:2502.15882
> **Tags:** #trick #block-encoding #spectrum-amplification #quantum-walk #singular-value-estimation

## What it does
Block-encodes a rectangular "square root" Hamiltonian $H_{\text{sqrt}}$ directly, avoiding the $2\times$ overhead of Hermitian dilations. Enables [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|spectrum amplification]] with standard [[Phase Estimation as Eigenvalue Filter for Walk-Based Search|phase estimation]].

## The trick

Given an SOS Hamiltonian $H_{\text{SA}} = \sum_\alpha O_\alpha^\dagger O_\alpha$, define the rectangular operator:

$$H_{\text{sqrt}} = \sum_\alpha |\chi_\alpha\rangle \otimes O_\alpha$$

Block-encode it as $U = \text{Sel} \cdot \text{Prep}^\dagger$, where Sel applies controlled block-encodings of each $O_\alpha$ and Prep loads the weighted superposition $\sum_\alpha (\lambda_\alpha / \lambda_{\text{sqrt}}) |\chi_\alpha\rangle$.

The rectangular block-encoding maps between spaces of different dimension. Define two quantum walks:

$$W_1 = \text{Ref}_B \cdot U, \qquad W_2 = \text{Ref}_{aB} \cdot U^\dagger$$

$W_1$ maps $|0\rangle_{aB}|\psi_j\rangle \to e^{i\arccos(\sqrt{E_j}/\lambda_{\text{sqrt}})}|\tilde{\phi}_j^+\rangle$ (alternating between left and right singular spaces).

The key observation: the product $W_2 W_1$ has eigenphases $\arccos(2E_j/\lambda_{\text{sqrt}}^2 - 1)$, which is exactly a block-encoding of $2H_{\text{SA}}/\lambda_{\text{sqrt}}^2 - 1$ via the second Chebyshev polynomial $T_2$. This restores standard phase estimation on a fixed subspace.

Phase estimation on $W_2 W_1$ estimates $E_j$ with precision $\varepsilon$ using $O(\sqrt{E_j \lambda_{\text{sqrt}}^2}/\varepsilon)$ queries — the spectrum amplification advantage.

## When to reach for it
- Any time you have a Hamiltonian in sum-of-squares form and care about low-energy eigenvalues
- When the [[SOS Spectral Amplification|SOS gap]] $E_{\text{gap}} \ll \Lambda$ (otherwise the overhead isn't worth it)
- Replaces Hermitian dilation approaches ([[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo]], Zlokapa-Somma) with half the queries

## Complexity
Each walk step uses 2 queries to Sel and 2 queries to Prep. Total query complexity for estimating eigenvalue $E_j$ to precision $\varepsilon$: $O(\sqrt{E_j \cdot \sum_\alpha \lambda_\alpha^2}/\varepsilon)$.

## Caveat
Only useful when $\Delta_{\text{gap}} = E_{\text{gap}}/\lambda_{\text{sqrt}}^2 \ll 1$. If the SOS lower bound isn't tight, $E_{\text{gap}} \approx \Lambda$ and you get no improvement. The alternating walk structure means controlled-$U$ and controlled-$U^\dagger$ must both be available.

## Related notes
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]]
- [[SOS Spectral Amplification]]
- [[Frustration-Free Gap Amplification via Walk Operator]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Block-Encoding Composition Algebra]]
