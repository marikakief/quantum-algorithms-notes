# Sequential Disentanglement for MPS Tomography

> **Source:** Cramer, Plenio, Flammia, Somma, Gross, Bartlett, Landon-Cardinal, Poulin, Liu, arXiv:1101.4366
> **Tags:** #trick #MPS #tomography #state-reconstruction #quantum-circuits

## What it does
Recovers the MPS description of an unknown quantum state by sequentially applying local disentangling unitaries, each of which peels off one site from the chain. The result is the quantum circuit that prepared the state.

## The trick

For an MPS with bond dimension $D$, the reduced density matrix on $\kappa = \lceil\log_d D\rceil + 1$ sites has rank $\leq D < d^{\kappa-1}$. This means a $\kappa$-site unitary can rotate the first site into $|0\rangle$, disentangling it from the rest:

$$\hat{U}_1 |\phi\rangle = |0\rangle_1 \otimes |v\rangle_{2,\ldots,N}$$

The unitary is constructed from the eigenvectors of the local reduced density matrix $\hat{\sigma}$:

$$\hat{U} = \sum_{s,s'} |s\rangle_1 \otimes |s'\rangle_{2,\ldots,\kappa} \langle \phi_{sd^{\kappa-1}+s'+1}|_{1,\ldots,\kappa}$$

**Procedure:**
1. Perform local tomography on sites $1, \ldots, \kappa$
2. Find the low-rank subspace of $\hat{\sigma}$ and construct $\hat{U}_1$
3. Apply $\hat{U}_1$ — site 1 is now $|0\rangle$, set it aside
4. Move right: tomograph sites $2, \ldots, \kappa+1$, construct $\hat{U}_2$, repeat
5. After $N - \kappa + 1$ steps: $|0\rangle^{\otimes(N-\kappa+1)} \otimes |\eta\rangle$

The sequence $\{\hat{U}_i^{-1}\}$ and boundary state $|\eta\rangle$ give the MPS.

**Error bounds:** Each truncation/measurement error $\varepsilon_i$ accumulates linearly: $\||\phi\rangle - |\psi\rangle\| \leq \sum_i \varepsilon_i \leq N\varepsilon$. Each $\varepsilon_i$ is directly measurable from the data (population of non-zero states after disentangling).

## When to reach for it

- Verifying MPS state preparation on quantum hardware (ion traps, optical lattices)
- Benchmarking quantum simulators that target 1D systems
- When you need the full state description (not just observable estimates), and the state is expected to have bounded entanglement
- As a subroutine in adaptive quantum simulation where the current state is periodically reconstructed

## Complexity

- **Measurements:** $O(N)$ local tomographies, each on $\kappa = O(\log D)$ sites
- **Unitary operations:** $N - \kappa + 1$ unitaries, each on $\kappa$ sites
- **Classical processing:** $O(N \cdot d^{2\kappa})$ for eigendecomposition of each local density matrix
- **Total:** Linear in $N$ for fixed bond dimension $D$

## Caveat

- Requires coherent unitary control of $\kappa$ neighbouring qudits — challenging in many experimental platforms. The companion local-measurement scheme avoids this at the cost of more complex classical post-processing.
- Only recovers the MPS to accuracy $N\varepsilon$, which can be loose for long chains with noisy measurements.
- If the true state is far from an MPS, the truncation errors will be large — but this is detectable from the measured $\varepsilon_i$.

## Related notes
- [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes]]
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Parent Hamiltonian Witness for State Certification]]
