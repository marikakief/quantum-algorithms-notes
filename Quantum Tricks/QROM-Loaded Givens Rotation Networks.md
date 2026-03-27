
> **Source:** Lee, Berry, Gidney, Huggins, McClean, Wiebe, Babbush, arXiv:2011.03494; builds on Kivlichan et al. arXiv:1711.04789 and von Burg et al. arXiv:2007.14460
> **Tags:** #trick #QROM #Givens-rotation #basis-rotation #qubitization #circuit-primitive

## What it does

Implements a coherently-controlled orbital basis rotation as part of a [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] SELECT oracle: for each LCU index $\ell$ in superposition, loads a different set of [[Givens Rotation Slater Determinant Preparation|Givens rotation]] angles from [[QROM (Quantum Read-Only Memory)|QROM]] and applies the corresponding basis transformation to the system register.

## The trick

In block-encoding constructions for quantum chemistry, the SELECT oracle often needs to rotate the system register into a term-dependent basis before applying a diagonal operator. Both double factorization (von Burg et al.) and the [[THC Non-Orthogonal Diagonal Coulomb Representation]] require this.

**Construction:**

1. The LCU index register $|\ell\rangle$ is in superposition (prepared by [[Coherent Alias Sampling for PREPARE|PREPARE]]).
2. Use [[QROM (Quantum Read-Only Memory)|QROM]] (or QROAM with $\sqrt{\Gamma}$ space-time tradeoff) to load the rotation angles $\{\theta^{(\ell)}_1, \ldots, \theta^{(\ell)}_K\}$ for index $\ell$. Here $K = O(N)$ angles define the [[Givens Rotation Slater Determinant Preparation|Givens rotation]] decomposition of the basis transformation for term $\ell$.
3. Apply the Givens rotation network to the $N/2$ system qubits (per spin sector), using the loaded angles from a quantum register. Each Givens rotation is a nearest-neighbour two-qubit gate parametrized by the loaded angle.
4. Apply the diagonal operator (e.g., $n_\mu n_\nu$).
5. Reverse the Givens rotation network (inverse basis transformation).
6. Uncompute the QROM (measure and correct).

**Cost per walk step:** The QROM load costs $O(\sqrt{\Gamma})$ Toffolis where $\Gamma$ is the total data size ($O(N^2)$ for THC, $O(N^2 \Xi)$ for double factorization). The Givens rotation application costs $O(b_\theta N)$ Toffolis where $b_\theta$ is the angle precision in bits. Together with [[Rotation Angle Coarse-Graining for Block Encodings|coarse-graining]], $b_\theta$ can be kept modest (16–20 bits for chemical accuracy).

**Key difference from static Givens circuits:** In standard [[Givens Rotation Slater Determinant Preparation|Slater determinant preparation]], the rotation angles are classically known and compiled into fixed gates. Here, the angles are loaded coherently from QROM and applied as parametrized rotations — the circuit structure is fixed but the angles vary in superposition over the index register.

## When to reach for it

- Any qubitization-based chemistry algorithm where the block encoding requires term-dependent basis rotations (double factorization, THC, or future tensor decompositions)
- When the basis rotations can be decomposed into $O(N)$ nearest-neighbour Givens rotations (true for general single-particle unitaries via QR decomposition)

## Complexity

- QROM load: $O(\sqrt{\Gamma})$ Toffolis + $O(\sqrt{\Gamma})$ ancilla
- Givens rotation application: $O(b_\theta N)$ Toffolis per pass (two passes per step for THC: one per index $\mu, \nu$)
- Total per step: $O(\sqrt{\Gamma} + b_\theta N)$
- For THC ($\Gamma = O(N^2)$): $O(N + b_\theta N) = O(b_\theta N) = \widetilde{O}(N)$

## Caveat

The Givens rotation synthesis from loaded angle registers requires rotation synthesis circuits (e.g., CORDIC or Hamming-weight phasing), which add constant-factor overhead proportional to $b_\theta$. The double-pass structure (rotate in, apply, rotate out) means you pay the rotation cost twice per walk step. This is the dominant constant-factor penalty of the THC approach relative to algorithms that don't require per-term basis changes.

## Related notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Rotation Angle Coarse-Graining for Block Encodings]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
