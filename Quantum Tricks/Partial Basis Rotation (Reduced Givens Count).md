# Partial Basis Rotation (Reduced Givens Count)

> **Source:** Motta, Ye, McClean, Li, Minnich, Babbush, Chan, arXiv:1808.02625
> **Tags:** #trick #circuit-compilation #Givens-rotation #low-rank #basis-rotation #linear-connectivity

## What it does

When a single-particle basis rotation $U^{(\ell)}$ is known to have only $\rho_\ell < N$ significant columns (from a low-rank approximation), implements the rotation using $N\rho_\ell - \rho_\ell(\rho_\ell + 1)/2$ [[Givens Rotation Slater Determinant Preparation|Givens rotations]] instead of the full $\binom{N}{2}$.

## The trick

In the [[Double Factorization of Two-Electron Integrals|double factorization]] scheme, each Cholesky factor $L^{(\ell)}$ is diagonalized, and only the $\rho_\ell$ largest eigenvalues are kept. The corresponding eigenvectors form an $N \times \rho_\ell$ partial unitary $\bar{U}^{(\ell)}$ (the first $\rho_\ell$ columns of the full $U^{(\ell)}$).

Apply the Thouless theorem / QR decomposition: decompose $\bar{U}^{(\ell)}$ into nearest-neighbor Givens rotations that zero out the sub-diagonal elements. Since $\bar{U}^{(\ell)}$ is only $N \times \rho_\ell$, the QR decomposition produces at most

$$N\rho_\ell - \frac{\rho_\ell(\rho_\ell + 1)}{2} = \binom{N}{2} - \binom{N - \rho_\ell}{2}$$

Givens rotations (the number of sub-diagonal elements that need zeroing). Compare to $\binom{N}{2}$ for a full $N \times N$ rotation.

For inter-basis rotations $\tilde{U}^{(\ell)} = U^{(\ell-1)} U^{(\ell)\dagger}$, the cost is determined by $\rho_{\ell+1}$ since the product can be composed into a single unitary operation.

## When to reach for it

- Any time you're implementing a sequence of basis rotations from a low-rank decomposition (double factorization, sum-of-squares compilation)
- When the eigenvalue spectrum of your one-body operator decays rapidly and you can truncate early
- Inside [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|greedy unitary compression]] circuits, where each factor's basis rotation may have rank $\ll N$

## Complexity

- **Givens rotations:** $N\rho_\ell - \rho_\ell(\rho_\ell + 1)/2$
- **Circuit depth:** $(N + \rho_\ell)/2$ on a linear chain
- **Savings vs full rotation:** reduces from $\binom{N}{2}$ to $\sim N\rho_\ell$ when $\rho_\ell \ll N$

## Caveat

The savings are proportional to $\rho_\ell / N$. If the eigenvalue spectrum doesn't decay (all eigenvalues significant), $\rho_\ell = N$ and there's no improvement. In the [[Double Factorization of Two-Electron Integrals|Hamiltonian double factorization]], $\langle \rho_\ell \rangle = O(\log N)$ empirically, giving large savings. For the [[Unitary Coupled Cluster (UCC) Ansatz|uCC operator]], $\langle \rho_{\ell,\mu} \rangle = O(N)$, so this trick provides no asymptotic advantage.

## Related notes
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[QROM-Loaded Givens Rotation Networks]]
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]]
