
> **Source:** Berry, Gidney, Motta, McClean, Babbush, arXiv:1902.02134
> **Tags:** #trick #quantum-chemistry #Hamiltonian-decomposition #low-rank #qubitization #LCU #Cholesky-decomposition

## What it does
Reduces the number of distinct LCU coefficients for an arbitrary-basis chemistry Hamiltonian from $O(N^4)$ to $O(N^3)$ by eigendecomposing the Coulomb supermatrix, enabling qubitization with $\widetilde{O}(N^{3/2}\lambda)$ Toffoli complexity instead of $\widetilde{O}(N^2\lambda)$.

## The trick
The two-electron integrals $V_{pqrs}$ (in chemist notation) can be reshaped into an $N^2/4 \times N^2/4$ real symmetric positive semidefinite matrix $W$ with composite indices $pq$ and $rs$. This is the same matricization used in [[Double Factorization of Two-Electron Integrals|double factorization]] and density fitting. Eigendecompose $W$:

$$W = \sum_{\ell=1}^{L} \omega_\ell\, g^{(\ell)} (g^{(\ell)})^T, \quad \omega_\ell \geq 0$$

The rank $L = O(N)$ empirically (physically: pairwise Coulomb interaction). The two-electron operator becomes:

$$V = \sum_{\ell=1}^{L} \omega_\ell \left(\sum_{p,q,\sigma} g^{(\ell)}_{pq}\, a^\dagger_{p,\sigma} a_{q,\sigma}\right)^2$$

For [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]], this means the PREPARE oracle needs to load $O(N^2 L) = O(N^3)$ distinct weights (with eightfold symmetry: $(L+1)(N^2/8 + N/4)$ unique values). The state preparation splits into:
1. Prepare superposition over $\ell$ (cost $O(L)$)
2. Prepare $(p,q)$ superposition conditioned on $\ell$ (cost dominated by [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] on $O(N^3)$ entries)
3. Prepare $(r,s)$ superposition conditioned on $\ell$ (same cost)

The [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] cost for $d = O(N^3)$ entries is $O(N^{3/2}\sqrt{M})$, giving total per-step cost $O(N^{3/2}\sqrt{\log N})$.

**Critical detail:** The positive semidefiniteness and low rank hold only in chemist ordering. Physicist ordering ($V_{pqrs}$ with $a^\dagger_p a^\dagger_q a_r a_s$) gives a full-rank matrix, and spin-orbital-level symmetry reduction also destroys the structure.

## When to reach for it
- Qubitizing an arbitrary-basis chemistry Hamiltonian where plane-wave structure is unavailable
- When you want the simplest factorization (single diag. of $W$) without the complexity of THC or double factorization
- When you have $N$ dirty ancilla qubits available from the system register

## Complexity
- PREPARE cost per walk step: $O(N^{3/2}\sqrt{\log(N/\varepsilon)})$ Toffolis (many-ancilla variant)
- Total phase estimation: $\widetilde{O}(N^{3/2}\lambda/\Delta E)$ Toffolis
- Space: $O(N)$ (dirty ancilla variant) or $O(N^{3/2})$ (many-ancilla variant)

## Caveat
The LCU 1-norm $\lambda_W$ from the factorized form is strictly $\geq \lambda_V$ from the original integrals (triangle inequality). Numerically, $\lambda_W / \lambda_V \approx O(N^{0.3})$. This overhead is compensated by the $\sqrt{N}$ improvement in QROAM cost, but means the factorized approach becomes relatively worse for very large systems.

Also: the single factorization does not exploit the $O(\log N)$ eigenvalue rank within each $g^{(\ell)}$ matrix (which [[Double Factorization of Two-Electron Integrals|double factorization]] does for Trotter). THC-based qubitization ([[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]]) effectively achieves a version of this second factorization within the LCU framework.

## Related notes
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Coherent Alias Sampling for PREPARE]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
