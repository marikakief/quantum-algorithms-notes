
> **Tags:** #trick #low-energy #phase-estimation #block-encoding
> **Source:** arXiv:2505.01528 (SOSSA)
> **Full notes:** [[SOSSA — Sum-of-Squares Spectral Amplification]]

## What it does
Reduces query complexity for low-energy simulation from $\lambda/\varepsilon$ to $\sqrt{\Delta\lambda}/\varepsilon$, where $\Delta$ is the shifted energy above the SOS lower bound, not necessarily the physical excitation gap.

## The trick
1. Solve an SDP to find a shifted SOS form:
   $$H + \beta\mathbb{1} = \sum_j B_j^\dagger B_j,$$
   with $-\beta$ a lower bound on $E_0$.
2. Build the rectangular square-root map:
   $$A = \sum_j |j\rangle \otimes B_j,\qquad A^\dagger A = H + \beta\mathbb{1}.$$
3. Block-encode $A$ and form the rectangular-walk product whose phases depend on the squared singular values.
4. Run phase estimation on that walk, not directly on a Hermitian operator $A$, to resolve low shifted energies with fewer queries.

## When to reach for it
- Low-energy physics (ground state, low-lying excitations)
- Hamiltonians where the SOS relaxation gives a tight bound (frustrated spin systems, SYK)
- Any setting where the ordinary LCU normalization $\lambda_{\rm LCU}$ is large but the relevant shifted energy scale is small

## Complexity
$O(\sqrt{\Delta_{\text{SOS}} \cdot \lambda_{\text{SOS}}}/\varepsilon)$ queries for a favourable SOS lower-bound gap and block-encoding normalization. Classical SDP preprocessing: polynomial in the chosen operator-algebra dimension, with numerical conditioning and certification costs.

## Caveat
Higher-degree SOS tightens $\Delta$ but grows $\lambda$ (more terms in the SOS decomposition). The *product* $\Delta \cdot \lambda$ determines the win — degree must be optimised per system. For SYK specifically, degree-2 Majorana SOS achieves $\sqrt{\Delta_{\rm SOS}\lambda_{\rm SOS}} \sim N^{3/2}$ vs ordinary $\lambda_{\rm LCU} \sim N^2$ — a $\sqrt{N}$ speedup. Whether higher degree helps depends on whether the tighter $\Delta$ compensates for the larger $\lambda$; the paper finds degree-2 sufficient for the SYK demonstration.

## Related Paper Notes

- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes]] — the full SOSSA theory paper: self-contained proofs, adaptive algorithms, SYK demonstration, query-optimal lower bounds
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]] — informal paper note on the same paper
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — chemistry-specific implementation with DFTHC
- [[Rectangular Block-Encoding for Spectrum Amplification]]
- [[SOS Decomposition via Semidefinite Programming for Chemistry]]
- [[DFTHC Factorization]]
- [[Adaptive Spectral-Amplified Energy Estimation]] — the adaptive binary search algorithm from the SOSSA paper
- [[SOS Lower Bound via Gram Matrix SDP]] — the classical SDP preprocessing step
- [[Majorana SOS with Double Factorization]] — block-encoding compilation for fermionic SOS generators
