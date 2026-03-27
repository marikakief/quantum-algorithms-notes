# Majorana SOS with Double Factorization

> **Source:** King, Low, Babbush, Somma, Rubin, arXiv:2505.01528 (Appendix D)
> **Tags:** #trick #SOS #double-factorization #Majorana #block-encoding #fermionic

## What it does

Efficiently block-encodes the [[SOS Spectral Amplification|SOSSA]] Hamiltonian for degree-2 Majorana sum-of-squares generators by diagonalising the quadratic part of each $B_j$ via Gaussian unitaries. This reduces the [[Block-Encoding Composition Algebra|block-encoding]] normalisation $\lambda_{\text{SOS}}$ compared to direct LCU compilation of the $B_j$ operators.

## The trick

After solving the [[SOS Lower Bound via Gram Matrix SDP|SOS SDP]], each generator is:

$$B_j = e_j\mathbb{1} + \sum_a f_{j,a}\gamma_a + \sum_{a,b} g_{j,ab}\gamma_a\gamma_b$$

The quadratic part $\sum_{ab} g_{j,ab}\gamma_a\gamma_b$ involves an antisymmetric matrix $g_{j,ab}$. Any real antisymmetric matrix can be block-diagonalised by an orthogonal transformation — and orthogonal transformations on Majorana operators correspond to **Gaussian unitaries** $U_j$ implementable as circuits of Givens rotations:

$$\sum_{ab} g_{j,ab}\gamma_a\gamma_b = U_j^\dagger \left(\sum_a \tilde{g}_{j,a}\,\gamma_{2a-1}\gamma_{2a}\right) U_j$$

(Separately for real and imaginary parts of $g_{j,ab}$.) After this decomposition:

$$B_j = e_j\mathbb{1} + \sum_a f_{j,a}\gamma_a + U_j^{(R)\dagger}\left(\sum_a \tilde{g}^{(R)}_{j,a}\gamma_{2a-1}\gamma_{2a}\right)U_j^{(R)} + U_j^{(I)\dagger}\left(\sum_a i\tilde{g}^{(I)}_{j,a}\gamma_{2a-1}\gamma_{2a}\right)U_j^{(I)}$$

The [[Block-Encoding Composition Algebra|block-encoding]] normalisation becomes:

$$\lambda_{\text{SOS}} = \sum_j \left(|e_j| + \|f_j\|_1 + \|\tilde{g}^{(R)}_j\|_1 + \|\tilde{g}^{(I)}_j\|_1\right)^2$$

This is bounded via Cauchy-Schwarz by:

$$\lambda_{\text{SOS}} \leq 4N \cdot \overline{\text{Tr}}(H + \beta\mathbb{1}) = 4N\beta$$

where $\overline{\text{Tr}}$ denotes the normalised trace. For SYK: $\beta = O(N)$ gives $\lambda_{\text{SOS}} = O(N^2)$, matching the LCU scaling per query — so the [[SOS Spectral Amplification|SOSSA]] query reduction carries through to end-to-end gate cost.

## When to reach for it

- Compiling SOSSA block-encodings for any fermionic Hamiltonian with degree-2 Majorana SOS generators
- The [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|SYK model]] specifically — this is the compilation strategy analysed in the paper
- Any setting where the SOS generators contain Majorana quadratic terms that benefit from [[Double Factorisation for Block-Encoding|double factorization]]

## Complexity

Gate cost per block-encoding: $O(N^4)$ for SYK (dominated by the number of terms $\sim N^4$). Each Gaussian unitary costs $O(N^2)$ gates, and there are $O(N^2)$ generators, so total gate cost is $O(N^4)$.

## Caveat

The Gaussian unitary diagonalisation only applies to the *quadratic* part. If the SOS generators are higher-degree (e.g., cubic Majorana monomials from a degree-3 SOS), this trick doesn't directly apply — the higher-degree terms would need a different compilation strategy, and the block-encoding normalisation would likely be worse.

## Related notes

- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes]] — source paper
- [[SOS Lower Bound via Gram Matrix SDP]] — the SDP that produces the generators compiled here
- [[SOS Spectral Amplification]] — the framework this trick serves
- [[Double Factorisation for Block-Encoding]] — the same diagonalisation idea applied in quantum chemistry
- [[DFTHC Factorization]] — extended double factorization used in the companion chemistry paper
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — prior SYK simulation (LCU-based)
- [[Asymmetric Qubitization]] — alternative SYK block-encoding approach
