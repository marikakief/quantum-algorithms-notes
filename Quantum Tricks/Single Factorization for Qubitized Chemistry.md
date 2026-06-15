
> **Source:** Berry, Gidney, Motta, McClean, Babbush, arXiv:1902.02134
> **Tags:** #trick #quantum-chemistry #Hamiltonian-decomposition #low-rank #qubitization #LCU #Cholesky-decomposition

## What it does
Reduces the number of distinct LCU coefficients for an arbitrary-basis chemistry Hamiltonian from $O(N^4)$ to $O(N^3)$ by eigendecomposing the Coulomb supermatrix, enabling phase estimation with $\widetilde{O}(N^{3/2}\lambda/\Delta E)$ Toffoli complexity instead of $\widetilde{O}(N^2\lambda/\Delta E)$ in the same cost model.

## The trick
The two-electron integrals $V_{pqrs}$ (in chemist notation) can be reshaped into an $N^2/4 \times N^2/4$ real symmetric positive semidefinite matrix $W$ with composite indices $pq$ and $rs$. This is the same matricization used in [[Double Factorization of Two-Electron Integrals|double factorization]] and density fitting. Eigendecompose $W$:

$$W = \sum_{\ell=1}^{L} \omega_\ell\, g^{(\ell)} (g^{(\ell)})^T, \quad \omega_\ell \geq 0$$

The useful rank $L = O(N)$ empirically for the molecular systems studied (physically: pairwise Coulomb interaction), but this is not a theorem for arbitrary integral tensors. The two-electron operator becomes:

$$V = \sum_{\ell=1}^{L} \omega_\ell \left(\sum_{p,q,\sigma} g^{(\ell)}_{pq}\, a^\dagger_{p,\sigma} a_{q,\sigma}\right)^2$$

For [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]], this means the PREPARE oracle needs to load $O(N^2 L) = O(N^3)$ distinct weights (with eightfold symmetry: $(L+1)(N^2/8 + N/4)$ unique values). The state preparation splits into:
1. Prepare superposition over $\ell$ (cost $O(L)$)
2. Prepare $(p,q)$ superposition conditioned on $\ell$ (cost dominated by [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] on $O(N^3)$ entries)
3. Prepare $(r,s)$ superposition conditioned on $\ell$ (same cost)

The [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] cost for $d = O(N^3)$ entries is $O(N^{3/2}\sqrt{M})$ for output width/precision parameter $M$, giving the usual $\widetilde{O}(N^{3/2})$ per-step shorthand only after suppressing these precision and output-size factors.

**Critical detail:** The positive semidefiniteness and low rank hold only in chemist ordering. Physicist ordering ($V_{pqrs}$ with $a^\dagger_p a^\dagger_q a_r a_s$) gives a full-rank matrix, and spin-orbital-level symmetry reduction also destroys the structure.

## When to reach for it
- Qubitizing an arbitrary-basis chemistry Hamiltonian where plane-wave structure is unavailable
- When you want the simplest factorization (single diag. of $W$) without the complexity of THC or double factorization
- When you have compatible dirty ancilla qubits available; some variants reuse system-register qubits as dirty workspace only when the SELECT/PREPARE schedule allows it

## Complexity
- PREPARE cost per walk step: $O(N^{3/2}\sqrt{\log(N/\varepsilon)})$ Toffolis (many-ancilla variant)
- Total phase estimation: $\widetilde{O}(N^{3/2}\lambda/\Delta E)$ Toffolis
- Space: $O(N)$ (dirty ancilla variant) or $O(N^{3/2})$ (many-ancilla variant)

## Caveat
The LCU 1-norm $\lambda_W$ from factorizing before taking absolute values is an LCU 1-norm tax relative to the direct sparse-integral norm $\lambda_V$ in the benchmarked decompositions. Numerically, $\lambda_W / \lambda_V \approx O(N^{0.3})$. This overhead is compensated by the $\sqrt{N}$ improvement in QROAM cost in the paper's target regimes, but it is not free.

Also: the single factorization does not exploit the $O(\log N)$ eigenvalue rank within each $g^{(\ell)}$ matrix (which [[Double Factorization of Two-Electron Integrals|double factorization]] does for Trotter). Later double-factorized qubitization and THC-based qubitization ([[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]]) exploit related additional structure, but THC is a different tensor ansatz rather than simply "single factorization plus one more eigendecomposition."

## Related notes
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Coherent Alias Sampling for PREPARE]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
