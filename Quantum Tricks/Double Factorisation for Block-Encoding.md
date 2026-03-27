
> **Tags:** #trick #block-encoding #fermionic #quantum-chemistry
> **Source:** [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush et al. (2019)]] "Quantum simulation of chemistry with sublinear scaling in basis set size," arXiv:1902.02134; used in arXiv:2505.01528 (SOSSA) for degree-2 Majorana SOS operators

## What it does
Efficiently block-encodes operators that are quadratic in fermion operators by diagonalising the coefficient matrix via basis rotations.

## The trick
Given $B = \sum_{ab} g_{ab} \gamma_a \gamma_b$ (quadratic in Majorana operators), the antisymmetric matrix $g$ can be brought to block-diagonal form by a Gaussian unitary $U$ (fermionic basis rotation):

$$B = U^\dagger \left(\sum_a \tilde{g}_a \gamma_{2a-1}\gamma_{2a}\right) U$$

The diagonal form is a sum of commuting number operators — easy to exponentiate. For operators with both linear and quadratic parts, split into real/imaginary parts and diagonalise each separately → at most 2 basis rotations.

## When to reach for it
- [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] Hamiltonians with two-body interactions (chemistry, condensed matter)
- Any operator that's quadratic in creation/annihilation or Majorana operators
- When the SOS $B_j$ operators are degree-2 Majorana polynomials

## Complexity
$O(N^2)$ gates per basis rotation (Givens rotation network). For $R$ terms: $O(R \cdot N^2)$ total.

## Caveat
Only helps for quadratic (degree-2) operators. Higher-degree $B_j$ need different decomposition strategies.

## Related Paper Notes

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]]
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]]
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — DFTHC generalizes DF via $(R, B, C)$ parameterization
- [[DFTHC Factorization]] — the generalization
