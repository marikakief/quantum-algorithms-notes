# Number Operator Symmetry Shifting for LCU 1-Norm Reduction

> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748 (2024); Loaiza, Khah, Wiebe, Izmaylov, QST 8, 035019 (2023)
> **Tags:** #trick #block-encoding #LCU #1-norm #symmetry #quantum-chemistry #fault-tolerant

## What it does

Reduces the LCU 1-norm $\lambda$ of a molecular Hamiltonian by up to $\sim 2\times$ by shifting the Hamiltonian with a function of the electron number operator $\hat{N}$. Since $\hat{N}$ commutes with $H$ (electron number is conserved), the shift doesn't change the eigenvalues within any fixed particle-number sector.

## The trick

Apply the shift:

$$H \to H - \frac{\alpha_2}{2}\hat{N}^2 - \left(\alpha_1 - \frac{\alpha_2}{2}\right)\hat{N}$$

where $\hat{N} = \sum_{p,\sigma} a^\dagger_{p,\sigma} a_{p,\sigma}$ is the total electron number operator. Since $\hat{N}|\Psi\rangle = N_e|\Psi\rangle$ for any state in the $N_e$-electron sector, the shifted Hamiltonian has eigenvalues $E_k - \alpha_2 N_e^2/2 - (\alpha_1 - \alpha_2/2)N_e$, which are just a known constant shift of the original eigenvalues.

**How to choose $\alpha_1, \alpha_2$:**

For double factorization (DF): the LCU 1-norm $\lambda_{\text{DF}}$ of the effective electron repulsion integral (ERI) tensor depends only on $\alpha_2$. First optimise $\alpha_2$ to minimise $\lambda_{\text{DF}}$ subject to the ERI tensor remaining positive semi-definite. Then choose $\alpha_1$ to minimise the 1-norm of the effective one-body part (which depends on both $\alpha_1$ and $\alpha_2$).

For THC: compute the symmetry shift as the median of $\{f_i\}$ where $f_i$ are eigenvalues of the one-body operator being block encoded. This is a simpler prescription that gives $\sim 1.5\times$ reduction.

**Concrete results:**

| System | Method | $\lambda$ (unshifted) | $\lambda$ (shifted) | Reduction |
|---|---|---|---|---|
| FeMoCo | THC | 1201.5 | 781.8 | $1.54\times$ |
| FeMoCo | DF | ~1171 | 582.4 | $\sim 2\times$ |

The DF reduction is more substantial because the quadratic $\hat{N}^2$ term directly cancels part of the two-body Coulomb repulsion, and DF preserves this structure more naturally than THC.

## When to reach for it

- Any [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based quantum chemistry simulation where the LCU 1-norm $\lambda$ dominates the QPE cost ($\lambda/\varepsilon$ walk steps).
- Particularly effective for large active spaces with many electrons, where the number operator shift can absorb a significant fraction of the two-body interaction.
- When using [[Double Factorization of Two-Electron Integrals|double factorization]] or [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]] block encodings — the shift is compatible with both.
- Can be combined with other $\lambda$-reduction techniques (integral truncation, basis optimisation).

## Complexity

Zero quantum overhead. The shift is applied classically to the Hamiltonian before constructing the block encoding. The only cost is the classical optimisation of $\alpha_1, \alpha_2$, which is negligible.

## Caveat

- The $\hat{N}^2$ term must keep the ERI tensor positive semi-definite after shifting. Over-aggressive $\alpha_2$ can break this, so there's a constraint on how much reduction is achievable.
- The technique works because electron number is exactly conserved. It doesn't apply to open systems or grand-canonical settings.
- The paper applies symmetry shifting only to the one-body component for THC (the authors note that further reduction is likely possible by incorporating the shift more deeply into the THC factorisation).

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Coherent Alias Sampling for PREPARE]]
