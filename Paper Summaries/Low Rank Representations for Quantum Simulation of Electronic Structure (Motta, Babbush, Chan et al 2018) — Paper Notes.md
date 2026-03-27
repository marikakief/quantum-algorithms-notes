> **Source:** Mario Motta, Erika Ye, Jarrod R. McClean, Zhendong Li, Austin J. Minnich, Ryan Babbush, Garnet Kin-Lic Chan, *Low rank representations for quantum simulation of electronic structure*, arXiv:1808.02625, npj Quantum Information **7**, 83 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/1808.02625) · [Journal](https://doi.org/10.1038/s41534-021-00416-z)
> **Tags:** #quantum-chemistry #Trotter #low-rank #Cholesky-decomposition #double-factorization #circuit-compilation #NISQ #fermionic-swap-network #UCC

---

## The computational problem

Given the second-quantized electronic structure Hamiltonian

$$H = \sum_{pq} h_{pq}\, a^\dagger_p a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs}\, a^\dagger_p a^\dagger_q a_r a_s$$

with $N$ spin-orbitals (and analogously, the [[Unitary Coupled Cluster (UCC) Ansatz|unitary coupled cluster]] operator $\tau = T - T^\dagger$), implement a single Trotter step $e^{i\Delta t H}$ with the fewest possible gates on a linearly connected qubit architecture. The standard approach incurs $O(N^4)$ gate complexity because $h_{pqrs}$ has $O(N^4)$ entries. The goal is to exploit structure in the integrals to reduce this.

## What the paper does

Introduces **double factorization** of the two-electron integrals: first a Cholesky decomposition (CD) of the two-electron integral supermatrix into $L$ vectors, then eigenvalue truncation (ET) of each Cholesky factor. The resulting Hamiltonian takes a nested pairwise form — a sum of $L$ terms, each of which is a number–number interaction $\sum_{ij} \lambda_i^{(\ell)} \lambda_j^{(\ell)} n_i^{(\ell)} n_j^{(\ell)}$ in a rotated orbital basis. Each term maps directly to a [[Givens Rotation Slater Determinant Preparation|Givens rotation]] basis change followed by an Ising-type interaction layer via a [[Fermionic Swap Network|fermionic swap network]], all on a linear chain.

The key numerical finding: for molecules of increasing size, $L = O(N)$ Cholesky vectors suffice and the average eigenvalue rank $\langle \rho_\ell \rangle = O(\log N)$. This gives Hamiltonian Trotter-step gate complexity $O(N^2 \log N)$ asymptotically, and $O(N^3)$ for fixed molecule with increasing basis. The uCCSD Trotter step scales as $O(N^3)$ with increasing basis but $O(N^4)$ with increasing molecule size (less favorable due to amplitude antisymmetry).

Concrete result: a chemically accurate Hamiltonian Trotter step for a 50-qubit molecular simulation requires ~4,000 layers of parallel two-qubit gates and fewer than $10^5$ non-Clifford rotations on a linear architecture. They also demonstrate the method on iron-sulfur clusters (up to the 146 spin-orbital nitrogenase P$_N$ cluster active space).

My assessment: this is the paper that introduced the double factorization idea to quantum computing, and it remains influential. The construction is clean and directly implementable. The $O(N^2 \log N)$ scaling is empirical — it relies on the observed $L = O(N)$, $\langle \rho_\ell \rangle = O(\log N)$ behavior, which is well-established numerically but not rigorously proven for general molecular systems. The perturbative error correction trick (shifting to correlation energies and adding first-order corrections) is practical and underappreciated. The uCC decomposition is less elegant — the antisymmetry of amplitudes forces $L = O(N^2)$ with system size, limiting the advantage.

## The algorithm / construction

### Step 1: Reorder to supermatrix form

Reindex the two-electron operator using the identity

$$V = \frac{1}{2}\sum_{pqrs} h_{ps,qr}\, a^\dagger_p a_s\, a^\dagger_q a_r - \text{one-body correction}$$

The reshaped coefficients $h_{ps,qr}$ form an $N^2 \times N^2$ real symmetric supermatrix (due to eightfold symmetry of the integrals).

### Step 2: First factorization (Cholesky decomposition)

Decompose the supermatrix via Cholesky decomposition:

$$h_{ps,qr} \approx \sum_{\ell=1}^{L} L^{(\ell)}_{ps}\, L^{(\ell)}_{qr}$$

Truncation: retain the smallest $L$ such that $\max_{ps,qr} |h_{ps,qr} - \sum_\ell L^{(\ell)}_{ps} L^{(\ell)}_{qr}| < \varepsilon$. Empirically, $L = O(N)$ for both increasing molecule size and increasing basis, with quite uniform behavior across different molecules at the same qubit count.

### Step 3: Second factorization (eigenvalue truncation)

Each $N \times N$ Cholesky factor matrix $L^{(\ell)}$ is real symmetric, so diagonalize it:

$$L^{(\ell)}_{ps} = \sum_{i=1}^{\rho_\ell} U^{(\ell)}_{pi}\, \lambda^{(\ell)}_i\, U^{(\ell)}_{si}$$

retaining only the $\rho_\ell$ eigenvalues with $\sum_{j > \rho_\ell} |\lambda^{(\ell)}_j| < \varepsilon$. The result is the **double-factorized** Hamiltonian:

$$H' = (h + S) + \sum_{\ell=1}^{L} V^{(\ell)}, \quad V^{(\ell)} = \sum_{ij} \frac{\lambda^{(\ell)}_i \lambda^{(\ell)}_j}{2}\, n^{(\ell)}_i\, n^{(\ell)}_j$$

where $n^{(\ell)}_i = a^\dagger_{\psi^{(\ell)}_i} a_{\psi^{(\ell)}_i}$ are number operators in the rotated basis defined by $U^{(\ell)}$.

### Step 4: Circuit implementation

The Trotter step becomes:

$$e^{i\Delta t H} \approx e^{i\Delta t(h+S)}\, U^{(1)\dagger}\, \prod_{\ell=1}^{L} e^{i\Delta t V^{(\ell)}}\, \tilde{U}^{(\ell)} + O(\Delta t^2)$$

where $\tilde{U}^{(\ell)} = U^{(\ell-1)} U^{(\ell)\dagger}$ is the inter-basis rotation.

Each layer consists of:
1. **Basis rotation** $\tilde{U}^{(\ell)}$: implemented via [[Givens Rotation Slater Determinant Preparation|Givens rotations]] from the QR decomposition. For an $N \times \rho_\ell$ partial rotation, this requires $N\rho_\ell/2 - \rho_\ell(\rho_\ell+1)/4$ Givens rotations (i.e. $\binom{N}{2} - \binom{N-\rho_\ell}{2}$), with depth $(N + \rho_\ell)/2$ on a linear chain.
2. **Pairwise interaction** $e^{i\Delta t V^{(\ell)}}$: diagonal $n_i n_j$ evolution implemented via [[Fermionic Swap Network|fermionic swap network]] in $\binom{\rho_\ell}{2}$ gates, depth $\rho_\ell$.

Exploiting $S_z$ symmetry (separate spin-up and spin-down rotations) halves the rotation count.

### Total gate counts

**Near-term (two-qubit gate count):**

$$\sum_{\ell=1}^{L} \left(\frac{N\rho_\ell}{4} + \frac{\rho_\ell^2}{4} - \rho_\ell\right)$$

on a linear architecture. Standard gate set (CZ/CNOT) costs $3\times$ this.

**Fault-tolerant (non-Clifford rotations):**

$$\sum_{\ell=1}^{L} \left(\frac{N\rho_\ell}{2} - 2\rho_\ell\right)$$

Each rotation synthesized with $\approx 2.3 \log(1/\varepsilon)$ T gates.

**Circuit depth:**

$$\sum_{\ell=1}^{L} \left(\frac{N}{2} + \frac{3\rho_\ell}{2}\right)$$

### Double factorization of the uCC operator

The uCC operator $\tau = T - T^\dagger$ has antisymmetric amplitudes ($t^{ab}_{ij} = -t^{ba}_{ij} = -t^{ab}_{ji}$), which prevents a direct symmetric supermatrix decomposition. Instead:

1. SVD of the amplitude tensor: $T_{ps,qr} = \sum_\ell \sigma_\ell U^{(\ell)}_{ps} V^{(\ell)}_{qr}$
2. Express $\tau$ as a sum of products $U_\ell V_\ell - V^\dagger_\ell U^\dagger_\ell$
3. Use the identity $X^2 - (X^\dagger)^2 = i\frac{1-i}{2}(X+iX^\dagger)^2 - i\frac{1+i}{2}(X-iX^\dagger)^2$ to rewrite $i\tau = \sum_{\ell,\mu} Y^2_{\ell,\mu}$ where $Y_{\ell,\mu}$ are normal operators
4. Diagonalize each $Y_{\ell,\mu}$ to get the same basis-rotation + diagonal-interaction circuit structure

The resulting scaling: $L = O(N)$ with increasing basis but $L = O(N^2)$ with increasing molecule size (the antisymmetry prevents the same rank compression as the Hamiltonian). The average eigenvalue rank $\langle \rho_{\ell,\mu} \rangle = O(N)$ with increasing molecule size (vs. $O(\log N)$ for the Hamiltonian).

## Key results

**Hamiltonian Trotter step (increasing molecule size):**
$$N_{\text{gates}} = O(N^2 \log N)$$
$$\text{Depth} = O(N^2)$$

**Hamiltonian Trotter step (fixed molecule, increasing basis):**
$$N_{\text{gates}} = O(N^3)$$

**uCCSD Trotter step (increasing molecule size):**
$$N_{\text{gates}} = O(N^4)$$

**uCCSD Trotter step (fixed molecule, increasing basis):**
$$N_{\text{gates}} = O(N^3)$$

**Concrete 50-qubit example:** ~4,000 layers of parallel nearest-neighbor gates; $< 10^5$ non-Clifford rotations per Trotter step.

**Iron-sulfur clusters:** Decomposition applied to [2Fe-2S] (40 spin-orbitals), [4Fe-4S] (72 spin-orbitals), and the nitrogenase P$_N$ cluster [8Fe-7S] (146 spin-orbitals). The P$_N$ cluster active space is larger than the FeMoCo active space used by Reiher et al.

## Comparison with prior work

| Method | Trotter step gates | Trotter step depth | Connectivity | Basis |
|---|---|---|---|---|
| Naive JW | $O(N^4)$ | $O(N^4)$ | All-to-all | Arbitrary |
| [[Fermionic Swap Network]] (Kivlichan et al. 2018) | $N(N-1)/2$ | $N$ | Linear | Arbitrary (dense $H$) |
| [[Plane-Wave Dual Basis]] Trotter (Babbush et al. 2018) | $O(N)$ per step | $O(N)$ | Planar | Plane-wave dual |
| **This paper (double factorization)** | $O(N^2 \log N)$ asympt. / $O(N^3)$ finite | $O(N^2)$ | **Linear** | **Arbitrary MO basis** |

The fermionic swap network gives $O(N^2)$ gates per Trotter step but treats all $O(N^2)$ pairwise terms. This paper reduces the number of terms that need simulation by exploiting the low-rank structure of the integrals. The plane-wave dual basis achieves even lower scaling but is limited to periodic systems; double factorization works in any Gaussian basis.

## Limits / caveats

1. **Scaling is empirical.** The $L = O(N)$ and $\langle \rho_\ell \rangle = O(\log N)$ scalings are well-supported numerically across many molecules but not rigorously proven. The crossover to asymptotic $O(N^2 \log N)$ behavior happens at larger $N$ than one might expect from $\langle \rho_\ell \rangle$ alone, because tails in the $\rho_\ell$ distribution matter.

2. **uCC scaling is worse.** The antisymmetry of coupled-cluster amplitudes prevents the same compression; $L = O(N^2)$ and $\langle \rho_{\ell,\mu} \rangle = O(N)$ with molecule size means the uCC Trotter step stays at $O(N^4)$ asymptotically. The authors acknowledge this and point to better decompositions for antisymmetric tensors as future work. The [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush (2021)]] greedy compression approach later addressed this.

3. **Trotter error not tightened.** The paper reduces the gate count per Trotter step but doesn't analyze the Trotter error of the truncated Hamiltonian $H'$ (vs. $H$). The truncation error and Trotter error interact — aggressive truncation may change the commutator structure.

4. **Only first-order Trotter.** Higher-order Trotter-Suzuki formulas would reduce the number of steps but increase the per-step cost. The interplay between double factorization and higher-order [[Product Formulas]]s isn't explored.

5. **Classical amplitudes as proxy for uCC.** The uCC amplitude data uses classical CCSD amplitudes (equal to uCC in the weak-coupling limit). For strongly correlated systems where uCC and classical CC amplitudes diverge significantly, the compression ratios may differ.

## Reusable ideas

1. **[[Double Factorization of Two-Electron Integrals]]** — The core technique: Cholesky decomposition of the integral supermatrix followed by eigenvalue truncation of each factor, reducing $O(N^4)$ terms to a sum over $L = O(N)$ pairwise interaction terms in rotated bases. Each term has circuit implementation via Givens rotations + fermionic swap network.

2. **[[Partial Basis Rotation (Reduced Givens Count)]]** — When only $\rho_\ell < N$ eigenvalues are retained, the basis rotation $U^{(\ell)}$ is an $N \times \rho_\ell$ partial unitary. Its QR decomposition requires only $N\rho_\ell - \rho_\ell(\rho_\ell+1)/2$ Givens rotations instead of $\binom{N}{2}$. Inter-basis rotations $\tilde{U}^{(\ell)} = U^{(\ell-1)} U^{(\ell)\dagger}$ can be composed into a single unitary at cost determined by $\rho_{\ell+1}$.

3. **[[Perturbative Error Correction for Truncated Hamiltonians]]** — Reduce truncation error by (a) measuring correlation energy $E'_c = E' - E'_{\text{HF}}$ rather than total energy (mean-field errors cancel), and (b) adding a first-order perturbative correction $\langle \psi | H - H' | \psi \rangle$ computed classically. Reduces the effective error from $O(\varepsilon)$ to $O(\varepsilon^2)$.

## References within this paper

- **Peng & Kowalski (2017)** — Introduced the nested matrix factorization (double factorization) in the classical chemistry context. The paper's entire quantum algorithm rests on adapting this classical decomposition.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean, Babbush et al. (2018)]] — The [[Fermionic Swap Network]] and [[Givens Rotation Slater Determinant Preparation|Givens rotation]] circuits that implement each double-factorization layer.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — Early VQE experiment cited as context for near-term simulation.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — UCC ansatz and Trotterized circuit strategies.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — Alternative basis approach (plane-wave dual) that avoids the $O(N^4)$ problem entirely for periodic systems.
- **Reiher, Wiebe, Svore, Wecker, Troyer (2017)** — FeMoCo resource estimates that this paper's iron-sulfur results compare against.
- **Jiang, Sung, Kechedzhi, Smelyanskiy, Boixo (2017)** — Swap-network-based circuit compilation strategies.
- **Seeley, Richard, Love (2012)** — [[Bravyi-Kitaev Transformation]] cited for alternative fermion encodings.

## Cross-links

- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — the FeMoCo benchmark whose Trotter cost this paper's double factorization technique helps reduce in subsequent qubitization-based approaches

### Paper notes
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — Directly builds on this paper's sum-of-squares decomposition; introduces greedy unitary compression as a better alternative to the SVD/Takagi decompositions used here for the uCC operator
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — Takes the single (first) factorization from this paper and applies it in the LCU/qubitization setting instead of Trotter; achieves $\widetilde{O}(N^{3/2}\lambda)$ Toffoli complexity for arbitrary bases; same author (Motta) on both papers
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC qubitization achieves better scaling ($\widetilde{O}(N)$ Toffolis per walk step) by using tensor hypercontraction instead of double factorization; the $\lambda_\zeta$ norm is comparable to $\lambda_{\text{DF}}$
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Provides the [[Fermionic Swap Network]] and [[Givens Rotation Slater Determinant Preparation]] primitives that this paper's circuits are built from
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Qubitization approach to the same problem; better asymptotic T complexity but requires more complex controlled operations than Trotter
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — Plane-wave dual basis gives $O(N)$-depth Trotter but only for periodic systems; double factorization is the arbitrary-basis analogue
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — UCC circuits that benefit from the double-factorization compilation of $\tau$
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — First-quantized alternative that avoids the two-electron integral problem entirely
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Repurposes the double factorization from this paper for [[Basis Rotation Grouping for VQE Measurement|measurement grouping]] rather than Trotter compilation, achieving $O(N)$ measurement circuits
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — Applies [[Double Factorization of Two-Electron Integrals|double factorization]] within DG blocks as an alternative Trotter decomposition (Appendix A); each block has bounded rank, so the per-block factorization runs in constant depth for large systems
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — applies double factorization in a condensed-phase (Hubbard + Coulomb) Trotter context; extends the circuit primitive to periodic geometries with full constant-factor resource estimates
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] — differentiates the double-factorized Hamiltonian to derive force block-encodings; empirically $\lambda_F^{(\text{DF})} \in O(N^{0.06})$ for H-chains vs. $\lambda_H^{(\text{DF})} \in O(N^{1.94})$, validating the DF structure for observables beyond the energy
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — extends double factorization to Bloch orbital basis for periodic solid-state materials; same two-electron integral factorization in a crystalline context
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — uses classical AFQMC with double-factorized and THC Hamiltonians as the competitive benchmark pushing back on claims of exponential quantum advantage for ground-state chemistry
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — applies double-factorized qubitization to CYP P450 as a pharmaceutical chemistry resource-estimation target; concrete DF integral counts for a realistic drug-relevant molecule
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]] — hardware experiments on correlated molecules using the same Hamiltonians whose efficient factorizations this paper establishes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — uses double-factorized block encoding for FeMoCo end-to-end QPE cost ($\lambda_{\text{DF}} = 582.4$ after symmetry shifting; $1.11 \times 10^{11}$ Toffolis at 95% confidence); DF is one of the two block-encoding strategies compared

### Trick cards
- [[Double Factorization of Two-Electron Integrals]] — The central technique of this paper
- [[Partial Basis Rotation (Reduced Givens Count)]] — Exploiting low-rank structure in basis rotations
- [[Perturbative Error Correction for Truncated Hamiltonians]] — Classical correction to reduce truncation error
- [[Fermionic Swap Network]] — Circuit primitive for pairwise $n_i n_j$ evolution
- [[Givens Rotation Slater Determinant Preparation]] — Circuit primitive for basis rotations
- [[Unitary Coupled Cluster (UCC) Ansatz]] — The uCC operator whose Trotter step is also compiled here
- [[THC Non-Orthogonal Diagonal Coulomb Representation]] — Alternative (and later, more efficient) factorization of the same integrals

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
