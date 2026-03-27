
> **Source:** Lee, Berry, Gidney, Huggins, McClean, Wiebe, Babbush, arXiv:2011.03494
> **Tags:** #trick #tensor-hypercontraction #block-encoding #quantum-chemistry #Coulomb-operator #non-orthogonal-basis

## What it does

Transforms the four-index Coulomb operator $V_{pqrs}$ in an arbitrary molecular orbital basis into a diagonal two-body operator ($n_\mu n_\nu$ terms only) by projecting into a larger, non-orthogonal basis defined by the THC leaf tensors. This gives the same diagonal structure as the [[Plane-Wave Dual Basis]] without requiring plane waves.

## The trick

Start from the tensor hypercontraction factorization:

$$V_{pqrs} \approx \sum_{\mu,\nu=1}^{M} \chi_p^{(\mu)} \chi_q^{(\mu)}\, \zeta_{\mu\nu}\, \chi_r^{(\nu)} \chi_s^{(\nu)}$$

Define $M$ non-orthogonal "THC orbitals" as linear combinations of the original orbitals:

$$\tilde{\phi}_\mu(\mathbf{r}) = \sum_p \chi_p^{(\mu)} \phi_p(\mathbf{r})$$

In this basis, the Coulomb operator becomes diagonal:

$$V \approx \frac{1}{2} \sum_{\alpha,\beta} \sum_{\mu \neq \nu} \zeta_{\mu\nu}\, \tilde{n}_{\mu,\alpha}\, \tilde{n}_{\nu,\beta}$$

The non-orthogonality is handled by writing the block encoding as an LCU where each term independently rotates the system register into the THC basis (via a [[Givens Rotation Slater Determinant Preparation|Givens rotation]] network with angles loaded from [[QROM (Quantum Read-Only Memory)|QROM]]), applies the diagonal operator, and rotates back. The overlap matrix of the non-orthogonal orbitals never appears explicitly.

The $\lambda$-norm of this representation ($\lambda_\zeta$) satisfies $\lambda_\zeta \leq \lambda_{\rm DF}$ in general, and sometimes beats $\lambda_{\rm DF}$ significantly. The information content is $\Gamma = O(N^2)$ regardless of how $N$ grows, giving $\widetilde{O}(N)$ Toffoli cost per [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] step.

**Why not directly qubitize THC?** Naively applying [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] to the standard THC form (without the non-orthogonal projection) gives a much larger $\lambda$ — sometimes worse than single factorization. The non-orthogonal diagonal form is what makes THC competitive.

## When to reach for it

- Simulating molecular electronic structure in Gaussian / molecular orbital bases where the Coulomb operator is dense ($V_{pqrs}$ has $O(N^4)$ nonzero entries)
- When you want [[Plane-Wave Dual Basis]]-like diagonal structure but can't afford the basis set overhead of plane waves for molecular systems
- When double factorization's $\widetilde{O}(N\sqrt{\Xi})$ space cost is too large

## Complexity

- $\widetilde{O}(N)$ Toffolis per walk step (matching plane-wave dual basis scaling)
- $\widetilde{O}(N)$ logical qubits
- Total: $\widetilde{O}(N\lambda_\zeta/\varepsilon)$ Toffolis for phase estimation
- Classical preprocessing: $O(N^4)$ or worse for the THC fit

## Caveat

Requires computing the THC factorization classically, which is a non-trivial $O(N^4)$ optimization. The THC rank $M$ is empirically $O(N)$ but this isn't rigorously proven for all molecular systems. If $M$ grows faster than $O(N)$, the advantage over double factorization shrinks. The per-step constant factor is higher than plane-wave dual qubitization because of the Givens rotation overhead.

## Related notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — extends THC to periodic systems via [[k-Point THC Factorization (Bloch Orbital THC)|k-THC]]; no asymptotic speedup from symmetry for THC due to unary iteration floor
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — [[Double Factorization of Two-Electron Integrals|Double factorization]], the predecessor technique that THC improves upon for fault-tolerant simulation
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Plane-Wave Dual Basis]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]]
- [[QROM-Loaded Givens Rotation Networks]]
- [[Rotation Angle Coarse-Graining for Block Encodings]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[k-Point THC Factorization (Bloch Orbital THC)]] — periodic extension of this trick
