
> **Source:** Takeshita, Rubin, Jiang, Lee, Babbush, McClean, arXiv:1902.10679
> **Tags:** #trick #orbital-optimization #MCSCF #active-space #NISQ #quantum-chemistry #RDM #postprocessing

## What it does

Lowers the energy of an active-space quantum calculation by classically optimizing one-body orbital rotations over the full orbital space (active + virtual + core), using only the measured 1- and 2-RDMs from the quantum device. No additional quantum resources required.

## The trick

After running [[Variational Quantum Eigensolver (VQE)|VQE]] (or any method) in an active space and measuring the 1-RDM ${}^1D$ and 2-RDM ${}^2D$, define the energy as a function of a unitary orbital rotation $U$:

$$E[U] = \sum_{ij} \tilde{h}_{ij}(U)\, {}^1D_{ji} + \frac{1}{2}\sum_{ijkl} \tilde{h}_{ijkl}(U)\, {}^2D_{klij}$$

where $\tilde{h}(U) = U h U^\dagger$ denotes the rotated integrals. The RDMs stay fixed; only the integrals change under $U$.

**Parameterization options:**
1. **Thouless:** $U = e^X$ with $X$ anti-Hermitian. Optimize the matrix elements of $X$ via Newton-Raphson or gradient descent.
2. **Givens rotations:** $U = \prod_{(i,b)} G_{i,b}(\theta_{i,b})$, a product of two-orbital rotations. Sweep over angles. Particularly natural for active↔virtual and active↔core pairs (the only non-redundant rotations when the active-space solution is internally optimal).

Minimize $E[U]$ classically using any optimizer (COBYLA, L-BFGS, Newton-Raphson). The variational principle guarantees $E[U^*] \leq E[\mathbb{I}]$.

**Iterative MCSCF variant:** Alternate between (a) solving the active-space problem on the quantum device with rotated integrals and (b) optimizing $U$ classically. This is the quantum analogue of CASSCF.

## When to reach for it

- You've run a VQE calculation in a restricted active space and want to squeeze out more accuracy for free (measurement-wise, you already have the 2-RDM from [[Pauli Expectation Value Estimation|energy estimation]]).
- As a cheap first pass before trying the more expensive [[Virtual Quantum Subspace Expansion (VQSE)|VQSE]].
- When implementing a quantum CASSCF loop: the orbital relaxation step is purely classical.
- Any time the active-space choice seems slightly off — orbital relaxation can partially compensate for a non-optimal active space selection.

## Complexity

- **Quantum cost:** Zero beyond the original active-space calculation. Only requires the 1- and 2-RDMs, which are already measured for energy estimation.
- **Classical cost:** Optimization over $O(N_A \cdot (N_V + N_C))$ rotation parameters. Each function evaluation is $O(N^4)$ (integral transformation). Modest for typical problem sizes.

## Caveat

- This is a local optimization over a non-convex landscape. For near-degenerate orbital rotations (common in transition-metal systems), convergence to local minima is a real risk.
- Orbital relaxation captures only one-body (orbital rotation) contributions to dynamic correlation. It misses genuine two-body virtual excitation effects that [[Virtual Quantum Subspace Expansion (VQSE)|VQSE]] can capture.
- The iterative MCSCF variant requires multiple rounds of quantum computation (re-solving the active-space problem with updated integrals), which may be expensive on quantum hardware.
- If the active-space reference is already variationally optimal with respect to orbital rotations (e.g., from a classical CASSCF initialization), single-step relaxation gives zero improvement. The benefit arises when orbitals haven't been optimized for the quantum reference state.

## Related notes
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]]
- [[Virtual Quantum Subspace Expansion (VQSE)]] — complementary technique; captures two-body virtual effects
- [[Active Space Restriction for VQE (CAS-UCC)]] — the base approximation being improved
- [[Variational Quantum Eigensolver (VQE)]] — produces the reference state and RDMs
- [[Givens Rotation Slater Determinant Preparation]] — same Givens rotation parameterization, different context
