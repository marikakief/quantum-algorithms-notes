
> **Source:** Childs, arXiv:0810.0312 (2010), §2; building on [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]]
> **Tags:** #trick #quantum-walk #qubitization-precursor #spectral #QSP-precursor #fundamental

## What it does

Maps any $N \times N$ Hermitian matrix $H$ to a discrete-time quantum walk unitary $U$ on $\mathbb{C}^N \otimes \mathbb{C}^N$ with the exact spectral relationship:

$$
H|\lambda\rangle = \lambda\|{\mathrm{abs}(H)}\||\lambda\rangle \implies U|\mu_\pm\rangle = \pm e^{\pm i\arcsin\lambda}|\mu_\pm\rangle
$$

Hamiltonian eigenvalues map to walk eigenphases via $\arcsin$. This is the bridge between quantum walks, [[Hamiltonian simulation]], and [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|quantum signal processing]].

## The construction

1. Let $|d\rangle$ be the principal (Perron-Frobenius) eigenvector of $\mathrm{abs}(H)$
2. Define the isometry $T: \mathbb{C}^N \to \mathbb{C}^N \otimes \mathbb{C}^N$ via states:

$$
|\psi_j\rangle = \frac{1}{\sqrt{\|\mathrm{abs}(H)\|}} \sum_k \sqrt{\frac{H^*_{jk}}{d_k} d_j}\, |j, k\rangle
$$

3. The walk unitary is:

$$
U = iS(2TT^\dagger - I)
$$

where $S|j,k\rangle = |k,j\rangle$ is the swap. Equivalently: reflect about $\mathrm{span}\{|\psi_j\rangle\}$, then swap registers.

**Inner products give $H$:** $\langle\psi_j|S|\psi_k\rangle = H_{jk}/\|\mathrm{abs}(H)\|$

## The spectral theorem (Theorem 1)

Each eigenvector $|\lambda\rangle$ of $H/\|\mathrm{abs}(H)\|$ produces two eigenvectors of $U$:

$$
|\mu_\pm\rangle = \frac{(1 - e^{\pm i\arccos\lambda})S}{\sqrt{2(1-\lambda^2)}} T|\lambda\rangle, \qquad \mu_\pm = \pm e^{\pm i\arcsin\lambda}
$$

The $\arcsin$ is exact — not an approximation. This is the key structural fact.

## When to reach for it

- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs's walk-Hamiltonian correspondence]]: simulate $e^{-iHt}$ via walk steps + phase estimation
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Qubitization]]: replace phase estimation with QSP phase factors to implement $P(\lambda)$ directly
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]: the general framework for polynomial transformations of block-encoded matrices
- Any setting where you need to convert a Hamiltonian (matrix) into a unitary whose eigenphases encode the eigenvalues

## Complexity

One walk step $U$ costs: one reflection $2TT^\dagger - I$ (implementing $T$ and $T^\dagger$) + one swap $S$. The reflection requires computing the states $|\psi_j\rangle$, which needs:
- The graph structure of $H$ (neighbors and weights)
- The principal eigenvector $|d\rangle$ of $\mathrm{abs}(H)$ (trivial for regular graphs; can be precomputed otherwise)

## Caveat

**The sign problem:** The normalisation uses $\|\mathrm{abs}(H)\|$ (entrywise absolute value), not $\|H\|$. When $H$ has negative off-diagonal entries, $\|\mathrm{abs}(H)\| > \|H\|$ and the simulation is correspondingly less efficient. For the glued-trees Hamiltonian, this gap is exponential.

Also requires $\mathrm{abs}(H)$ to be irreducible (connected graph). Reducible case: treat each component separately.

## Related notes

- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]]
- [[Linear-Time Hamiltonian Simulation via Walk Phase Estimation]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Block-Encoding Composition Algebra]]
