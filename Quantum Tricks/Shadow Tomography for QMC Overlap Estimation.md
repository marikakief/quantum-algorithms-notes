# Shadow Tomography for QMC Overlap Estimation

> **Source:** Huggins, O'Gorman, Rubin, Reichman, Babbush, Lee, arXiv:2106.16235
> **Tags:** #trick #shadow-tomography #QMC #AFQMC #measurement #NISQ

## What it does

Estimates the overlap $\langle \phi | \Psi_T \rangle$ between a quantum trial wavefunction $|\Psi_T\rangle$ and arbitrary walker wavefunctions $|\phi\rangle$, using a fixed set of pre-collected measurements — no iterative quantum-classical interaction needed.

## The trick

Prepare $|\tau\rangle = (|0\rangle + |\Psi_T\rangle)/\sqrt{2}$ on the quantum device. The overlap is:

$$\langle \phi | \Psi_T \rangle = 2\,\text{Tr}[|\tau\rangle\langle\tau| \cdot |0\rangle\langle\phi|]$$

Apply random $N$-qubit Clifford unitaries $U_k$ and measure in the computational basis to get outcomes $|b_k\rangle$. The classical shadow $\{U_k^\dagger |b_k\rangle\langle b_k| U_k\}$ allows estimating:

$$\langle \phi | \Psi_T \rangle = 2(2^N + 1)\,\mathbb{E}_k\!\left[\langle \phi | U_k^\dagger | b_k \rangle \langle b_k | U_k | 0 \rangle\right]$$

The number of measurement repetitions to estimate $M$ overlaps simultaneously to additive error $\varepsilon$ with failure probability $\delta$:

$$R = O\!\left(\frac{\log M - \log \delta}{\varepsilon^2}\right)$$

The logarithmic dependence on $M$ is the key property — you can reuse the same shadow for all walkers across all time steps of the QMC calculation. The quantum device runs once; the classical computer queries the shadow thousands of times.

**Partitioned variant:** Split qubits into spin sectors and use tensor products of smaller Cliffords. Reduces circuit depth from $O(N)$ to $O(N/2)$ at the cost of more measurements in practice. The inverse channel factorizes: $\mathcal{M}^{-1} = \bigotimes_p \mathcal{M}_{N_p}^{-1}$.

**Circuit depth:** Using the Bravyi-Maslov H-free Clifford decomposition, the shadow tomography circuits need at most $2n + 2$ CZ gate layers (vs $9n$ for a general Clifford). The circuits decompose into a CZ layer sandwiched by single-qubit gates.

## When to reach for it

- Any hybrid algorithm where a quantum state needs to be queried for overlaps or expectation values many times by a classical outer loop.
- When you want to decouple the quantum and classical computers (no real-time feedback).
- When the number of queries $M$ is large but the required per-query precision $\varepsilon$ is moderate.

Not useful when you need exponentially precise overlaps (the additive error guarantee becomes useless if the overlap itself is exponentially small).

## Complexity

- **Quantum:** $R = O(\log M / \varepsilon^2)$ state preparations + measurements. Each circuit has depth $O(N)$ for state prep + $O(N)$ for the Clifford measurement.
- **Classical post-processing:** For AFQMC walkers (Slater determinants), computing $\langle \phi | U_k^\dagger | b_k \rangle$ currently requires exponential enumeration of basis states. This is the major bottleneck. For Green's function QMC (walkers are computational basis states), the post-processing is efficient via Gottesman-Knill.

## Caveat

The exponential classical post-processing for Slater determinant walkers is a serious limitation that prevents scaling QC-AFQMC with this specific Clifford-based protocol. This bottleneck was resolved by [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Wan, Huggins, Lee, and Babbush (2022)]], who replaced random Clifford circuits with random [[Matchgate 3-Design for Classical Shadows|matchgate circuits]] (fermionic Gaussian unitaries). The resulting [[Pfaffian Shadow Estimation for Fermionic Observables|matchgate shadow]] post-processing runs in $O(n^4)$ per sample for overlap estimation, with sublinear-in-$n$ variance. The Hadamard test is an alternative that avoids the post-processing issue but reintroduces iterative quantum-classical coupling.

## Related notes

- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]]
- [[Pauli Expectation Value Estimation]] — alternative measurement approach for Hamiltonian expectation values
- [[Noise-Resilient Overlap Ratios in QC-QMC]] — the noise cancellation that makes this protocol practical
- [[Virtual Correlation via Matchgate Contraction]] — reduces the problem to active-space overlaps
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — resolves the exponential post-processing bottleneck via matchgate circuits
- [[Matchgate 3-Design for Classical Shadows]] — the design-theoretic result enabling matchgate shadows
- [[Gradient Encoding for Multi-Observable Estimation]] — alternative approach to multi-property estimation achieving $\tilde{O}(\sqrt{M}/\varepsilon)$ queries (vs shadow tomography's $O(\log M / \varepsilon^4)$); better in the high-precision regime
