
> **Source:** Lee, Berry, Gidney, Huggins, McClean, Wiebe, Babbush, arXiv:2011.03494; builds on Kivlichan et al. arXiv:1711.04789 and von Burg et al. arXiv:2007.14460
> **Tags:** #trick #QROM #Givens-rotation #basis-rotation #qubitization #circuit-primitive

## What it does

Implements a coherently-controlled orbital basis rotation as part of a [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] SELECT oracle: for each LCU index $\ell$ in superposition, loads a different set of [[Givens Rotation Slater Determinant Preparation|Givens rotation]] angles from [[QROM (Quantum Read-Only Memory)|QROM]] and applies the corresponding basis transformation to the system register.

## The trick

In block-encoding constructions for quantum chemistry, the SELECT oracle often needs to rotate the system register into a term-dependent basis before applying a diagonal operator. Both double factorization (von Burg et al.) and the [[THC Non-Orthogonal Diagonal Coulomb Representation]] require this.

**Construction:**

1. The LCU index register $|\ell\rangle$ is in superposition (prepared by [[Coherent Alias Sampling for PREPARE|PREPARE]]).
2. Use [[QROM (Quantum Read-Only Memory)|QROM]] (or QROAM with $\sqrt{\Gamma}$ space-time tradeoff) to load the rotation angles $\{\theta^{(\ell)}_1, \ldots, \theta^{(\ell)}_K\}$ for index $\ell$. In the THC construction, the selected basis changes use $K=O(N)$ angles per term; a generic single-particle unitary would require $O(N^2)$ Givens rotations.
3. Apply the Givens rotation network to the $N/2$ system qubits (per spin sector), using the loaded angles from a quantum register. Each Givens rotation is a nearest-neighbour two-qubit gate parametrized by the loaded angle.
4. Apply the diagonal operator (e.g., $n_\mu n_\nu$).
5. Reverse the Givens rotation network (inverse basis transformation).
6. Uncompute the loaded data either by a coherent inverse lookup or by [[Measurement-Based QROM Uncomputation]] when the angle registers are no longer needed coherently and the phase-fixup preconditions apply.

**Cost per walk step:** The QROM/QROAM load costs $O(\sqrt{\Gamma})$ Toffolis where $\Gamma$ is the total angle-data size ($O(N^2)$ for THC only when the empirical THC rank/precision assumptions hold, and larger for double-factorized variants). Loading the angles is only half the story: applying coherent rotations from angle registers costs $O(b_\theta N)$ Toffolis per pass in the THC setting, where $b_\theta$ is the angle precision in bits. Together with [[Rotation Angle Coarse-Graining for Block Encodings|coarse-graining]], $b_\theta$ can be kept modest (16–20 bits for chemical accuracy in the reported benchmarks), but it is a significant constant-factor cost.

**Key difference from static Givens circuits:** In standard [[Givens Rotation Slater Determinant Preparation|Slater determinant preparation]], the rotation angles are classically known and compiled into fixed gates. Here, the angles are loaded coherently from QROM and applied as parametrized rotations — the circuit structure is fixed but the angles vary in superposition over the index register.

## When to reach for it

- Any qubitization-based chemistry algorithm where the block encoding requires term-dependent basis rotations (double factorization, THC, or future tensor decompositions)
- When the selected basis rotations have an $O(N)$ nearest-neighbour Givens decomposition in the construction being used; generic dense single-particle unitaries require $O(N^2)$ rotations via QR decomposition

## Complexity

- QROM load: $O(\sqrt{\Gamma})$ Toffolis + $O(\sqrt{\Gamma})$ ancilla
- Givens rotation application: $O(b_\theta N)$ Toffolis per pass (two passes per step for THC: one per index $\mu, \nu$)
- Total per step: $O(\sqrt{\Gamma} + b_\theta N)$
- For THC with empirical $M=O(N)$ and controlled precision ($\Gamma = O(N^2)$): $O(N + b_\theta N) = O(b_\theta N) = \widetilde{O}(N)$

## Caveat

The Givens rotation synthesis from loaded angle registers requires rotation synthesis circuits (e.g., phase-gradient, Hamming-weight phasing, or CORDIC-style arithmetic), which add overhead proportional to $b_\theta$. The double-pass structure (rotate in, apply, rotate out) means you pay the rotation cost twice per selected basis change. This is the dominant constant-factor penalty of the THC approach relative to algorithms that use fixed compiled rotations or no per-term basis changes.

## Related notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Rotation Angle Coarse-Graining for Block Encodings]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
