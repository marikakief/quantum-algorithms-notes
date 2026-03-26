> **Source:** Ryan Babbush, Dominic W. Berry, Yuval R. Sanders, Ian D. Kivlichan, Artur Scherer, Annie Y. Wei, Peter J. Love, Alán Aspuru-Guzik, *Exponentially More Precise Quantum Simulation of Fermions in the Configuration Interaction Representation*, arXiv:1506.01029, Quantum Science and Technology **3**, 015006 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1506.01029) · [Journal](https://doi.org/10.1088/2058-9565/aa9463)
> **Tags:** #quantum-simulation #quantum-chemistry #CI-matrix #first-quantized #LCU #taylor-series #sparse-simulation #fermionic #gate-complexity

---

## The computational problem

Simulate time evolution under the molecular electronic Hamiltonian (Born-Oppenheimer approximation, fixed nuclei):

$$H = -\sum_j \frac{\nabla_{r_j}^2}{2} - \sum_{q,j} \frac{Z_q}{\|R_q - r_j\|} + \sum_{i < j} \frac{1}{\|r_i - r_j\|}$$

for $\eta$ electrons in a basis of $N$ single-particle spin-orbitals, for time $t$, to precision $\varepsilon$.

The algorithm works in the **configuration interaction (CI) representation**: the first-quantized sparse matrix of $H$ in the basis of Slater determinants ($\eta$-electron occupations of $N$ spin-orbitals). Matrix elements are given by the Slater-Condon rules and depend on one- and two-electron integrals $h_{ij}$ and $h_{ijkl}$.

The key point: the CI matrix has $\binom{N}{\eta}$ rows/columns but is extremely sparse (nonzero entries only for determinants differing by at most two orbitals), and its entries can be computed on-the-fly from molecular integrals rather than stored.

## What the paper does

It gives a quantum simulation algorithm for molecular Hamiltonians with gate complexity $\tilde{O}(\eta^2 N^3 t)$ and $\tilde{O}(\eta)$ qubits. The qubit count is exponentially better than second-quantized representations (which need $\tilde{O}(N)$ qubits), and the gate count beats prior algorithms when $\eta \ll N$. Precision scaling is $O(\log 1/\varepsilon)$ (logarithmic, not polynomial as in [[Trotterized Time Evolution for Chemistry]]).

This is the paper where the [[Truncated Taylor Series Simulation]] approach is applied to the first-quantized CI representation, enabling [[On-the-fly Molecular Integral Evaluation]] without a classical preprocessing step. The result is asymptotically the best scaling in the literature at the time of publication.

## The algorithm / construction

### 1. Represent the state in the CI basis

The $\eta$-electron state is stored as a sorted list of $\eta$ occupied orbital indices, each requiring $\log N$ bits:

$$|\psi\rangle = \sum_{\alpha \in \binom{[N]}{\eta}} c_\alpha |\alpha\rangle, \quad |\alpha\rangle = |i_1, i_2, \ldots, i_\eta\rangle,\; i_1 < i_2 < \cdots < i_\eta$$

Total qubits: $\eta \log N = \tilde{O}(\eta)$, compared to $N$ qubits for second-quantized representations. For molecules where $\eta \ll N$, this is exponentially fewer qubits.

### 2. Decompose $H$ into 1-sparse terms via graph edge-coloring

The CI matrix is sparse: each row $\alpha$ has nonzero entries only in rows $\beta$ that differ from $\alpha$ by 0, 1, or 2 orbitals (Slater-Condon rules). The matrix elements are:

- **Diagonal** ($\alpha = \beta$): $H_{\alpha\alpha} = \sum_{i \in \alpha} h_{ii} + \sum_{i < j \in \alpha} (h_{ijij} - h_{ijji})$
- **Single-excitation** ($\alpha, \beta$ differ by one orbital $i \to j$): $H_{\alpha\beta} = h_{ij} + \sum_{k \in \alpha \cap \beta} (h_{ikjk} - h_{ikkj})$
- **Double-excitation** ($\alpha, \beta$ differ by two orbitals): $H_{\alpha\beta} = \pm(h_{ijkl} - h_{ijlk})$

View the nonzero off-diagonal elements as edges of a graph on $\binom{N}{\eta}$ nodes. Use a graph edge-coloring to partition $H$ into a sum of $\Gamma = O(\eta^2 N^2)$ matrices $H_\gamma$, each of which is **1-sparse** (each row and column has at most one nonzero entry). Each color $\gamma$ is labeled by an 8-tuple of indices $(a_1, b_1, i, p, a_2, b_2, j, q)$ that uniquely and reversibly maps each row to its nonzero column.

### 3. Discretize integrals and decompose into self-inverse unitaries

Each 1-sparse matrix $H_\gamma$ has entries that are molecular integrals (or sums of them). Discretize using Riemann sums over a grid of $\mu$ points per dimension. Lemmas 1–3 of the paper bound $\mu$ in terms of orbital regularity parameters (maximum orbital value $\phi_\text{max}$, decay radius $x_\text{max}$, derivative bounds $\gamma_1, \gamma_2$) and the target discretization error $\delta$.

After discretization, decompose each 1-sparse term using a binary rounding trick: round matrix entries to multiples of $\zeta$, then split into $O(\|H_\gamma\|_\infty / \zeta)$ self-inverse 1-sparse unitaries with entries in $\{0, \pm 1\}$. The total number of such unitaries is

$$L = \Theta(\Gamma \cdot M) = \Theta\!\left(\eta^2 N^2 \cdot \frac{\max_\gamma \|H_\gamma\|_\infty}{\zeta}\right)$$

### 4. Build the select oracle

Construct two subroutines:
- **$Q_\text{col}$**: given color $\gamma$ and row $\alpha$, output the unique column $\beta$ (or diagonal indicator). Logic on $\eta \log N$ bits; cheap.
- **$Q_\text{val}$**: given color $\gamma$, grid point $\rho$, and row/column $\alpha, \beta$, output the (±1) matrix entry. Uses [[On-the-fly Molecular Integral Evaluation]]; cost $\tilde{O}(N)$ gates.

Combining these:

$$\text{select}(H)|{\ell}\rangle|\psi\rangle = |{\ell}\rangle H_\ell |\psi\rangle$$

where $\ell$ indexes into the list of $L$ self-inverse unitaries. Implementing $\text{select}(H)$ requires $\tilde{O}(N)$ gates (dominated by $Q_\text{val}$).

### 5. Simulate via truncated Taylor series

Apply the [[Truncated Taylor Series Simulation]] (Berry, Childs, Cleve, Kothari, Somma 2015) technique:

1. Split total time $t$ into $r$ segments of size $\tau = t/r$.
2. On each segment, approximate $e^{-iH\tau}$ by the truncated Taylor series to order $K$:

$$e^{-iH\tau} \approx \sum_{k=0}^{K} \frac{(-i\tau H)^k}{k!}$$

3. Substitute $H = \frac{\zeta}{w} \sum_\ell H_\ell$ (where $w$ is a normalization), expanding $H^k$ as a sum over $k$-tuples of unitary terms.
4. Implement as an LCU: prepare the superposition state $B|0\rangle$ over $(k, \ell_1, \ldots, \ell_k)$ tuples, apply controlled $H_{\ell_1} \cdots H_{\ell_k}$ via $k$ applications of $\text{select}(H)$, then postselect on $B^\dagger|0\rangle$.
5. Use oblivious amplitude amplification to remove the postselection cost.
6. Parameters: $K = O(\log 1/\varepsilon)$ and $r = O(\eta^2 N^2 \|\lambda\| t)$ where $\|\lambda\|$ is the 1-norm of the LCU coefficients.

The dominant cost per segment is $O(K)$ calls to $\text{select}(H)$, each costing $\tilde{O}(N)$ gates. Total gate count: $\tilde{O}(r \cdot K \cdot N) = \tilde{O}(\eta^2 N^3 t)$.

## Key results

**Theorem 1:** Under the orbital regularity assumptions (orbitals decay exponentially beyond radius $O(\log N)$, values and derivatives bounded by polylog in $N$, evaluation costs $\tilde{O}(1)$), the evolution $e^{-iHt}$ can be simulated within error $\varepsilon$ with:

$$\text{Gate count} = \tilde{O}(\eta^2 N^3 t), \qquad \text{Qubits} = \tilde{O}(\eta)$$

The precision scaling is $O(\log 1/\varepsilon)$ — polylogarithmic in $1/\varepsilon$, compared to polynomial for [[Trotterized Time Evolution for Chemistry]].

**Key comparison with prior algorithms:**

| Algorithm | Representation | Qubits | Gate count |
|---|---|---|---|
| Trotter, $p$-th order | Second-quantized | $\tilde{O}(N)$ | $\tilde{O}(N^{8+o(1)} t / \varepsilon^{o(1)})$ |
| Taylor series + database | Second-quantized | $\tilde{O}(N)$ | $\tilde{O}(N^4 \|H\| t)$ |
| Taylor series + on-the-fly | Second-quantized | $\tilde{O}(N)$ | $\tilde{O}(N^5 t)$ |
| **This work (CI matrix)** | **First-quantized** | $\tilde{O}(\eta)$ | $\tilde{O}(\eta^2 N^3 t)$ |

For $\eta \ll N$ (the typical chemistry regime), $\eta^2 N^3 \ll N^4$, giving a real improvement. For $\eta = O(\sqrt{N})$, the gate counts are comparable; for $\eta = O(N)$, the CI approach is worse.

**Precision scaling:** The [[Truncated Taylor Series Simulation]] gives only polylogarithmic dependence on $1/\varepsilon$ (via the truncation order $K = O(\log 1/\varepsilon)$), compared to Trotter's polynomial $O(1/\varepsilon)$ dependence. This is the "exponentially more precise" of the title.

## Comparison with prior work

| Paper | Year | Approach | Qubits | Gate complexity | Precision |
|---|---|---|---|---|---|
| Aspuru-Guzik et al. | 2005 | Trotter + QPE (2nd quantized) | $\tilde{O}(N)$ | $\tilde{O}(N^{11}t/\varepsilon)$ | $O(1/\varepsilon)$ |
| Wecker et al. | 2015 | Trotter (BK, 1st order) | $O(N)$ | $O(N^{8+o(1)}t/\varepsilon^{o(1)})$ | $O(1/\varepsilon)$ |
| Babbush et al. 2016 (NJP) | 2016 | Taylor series, 2nd-quantized, database | $O(N)$ | $\tilde{O}(N^4 \|H\| t)$ | $O(\log 1/\varepsilon)$ |
| Babbush et al. 2016 (NJP, on-the-fly) | 2016 | Taylor series, 2nd-quantized, on-the-fly | $O(N)$ | $\tilde{O}(N^5 t)$ | $O(\log 1/\varepsilon)$ |
| **This work** | **2018** | **Taylor series, CI matrix, on-the-fly** | $\tilde{O}(\eta)$ | $\tilde{O}(\eta^2 N^3 t)$ | $O(\log 1/\varepsilon)$ |

The connection to [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. (2016)]]: that paper cites an early version of this work (NJP 18, 033032) as the theoretical backing for the Taylor series simulation approach used there, even though the hardware experiment used Trotter. The current paper gives the full formal proof (v3 is a complete rewrite from 12 to 41 pages) and establishes the algorithm for larger-scale analysis.

The paper is a significant step toward algorithms for classically intractable molecules. The CI approach is more qubit-efficient than second-quantized methods, which matters for near-term devices. However, the gate count still scales as $\eta^2 N^3$ — getting to, say, $N = 100$ orbitals, $\eta = 10$ electrons gives $\sim 10^7$, which requires fault tolerance.

## Limits / caveats

- **Orbital regularity assumptions:** Theorem 1 requires the orbital basis functions to satisfy specific regularity conditions (exponential decay, bounded values and derivatives). Not all orbital choices satisfy these; plane-wave bases generally do, but standard Gaussian atomic orbital bases need care.
- **$\tilde{O}$ hides polylogarithms:** The actual gate count has polylogarithmic factors in $N$, $\eta$, $t$, $1/\varepsilon$, and the orbital parameters $\phi_\text{max}$, $x_\text{max}$. For small molecules, these can dominate.
- **$\eta \ll N$ regime:** The improvement over second-quantized methods requires $\eta \ll N$. For the minimal STO-6G basis of H₂ where $\eta = 2$, $N = 4$, the improvement is minimal.
- **CI vs. full CI:** The CI matrix here is the full CI matrix (all Slater determinants), not the truncated CI (CISD, CISDT) used in classical quantum chemistry. Full CI is exponentially large classically, which is why quantum simulation is interesting — but the circuit uses only $\tilde{O}(\eta)$ qubits because it works in the first-quantized picture.
- **Ground-state problem:** The algorithm simulates time evolution, not directly the ground-state energy. Ground-state estimation requires combining with phase estimation, which needs a good initial state (Hartree-Fock or better). For strongly correlated systems, the Hartree-Fock overlap can be small.
- **No experimental demonstration:** This is a purely theoretical algorithm; it predates the hardware implementations that would be needed to validate it. The qubit counts are still well beyond current devices for chemically interesting systems.
- **Comparison with QSVT:** Subsequent work using qubitization and QSVT (Berry, Babbush, et al., 2019) achieves better gate complexity for specific Hamiltonians. This paper's approach is superseded in some regimes.

## Reusable ideas

1. [[CI Matrix Simulation (First-Quantized Encoding)]] — Store the η-electron wavefunction as a list of occupied orbital indices ($\tilde{O}(\eta)$ qubits) rather than second-quantized occupation numbers ($\tilde{O}(N)$ qubits). Slice the CI Hamiltonian into 1-sparse Hermitian pieces via graph coloring; each piece acts on determinants differing by at most two orbitals.

2. [[Truncated Taylor Series Simulation]] — Simulate Hamiltonian evolution $e^{-iHt}$ by expanding in a truncated Taylor series and implementing the result as a linear combination of unitaries (LCU). Achieves $O(\log 1/\varepsilon)$ precision scaling (polylogarithmic), compared to Trotter's $O(1/\varepsilon)$.

3. [[On-the-fly Molecular Integral Evaluation]] — Instead of precomputing and storing all $O(N^4)$ molecular integrals classically (exponentially expensive for large bases), build a quantum oracle that computes individual integral values on-the-fly using a quantum arithmetic circuit. Gate cost per evaluation: $\tilde{O}(N)$. Avoids the classical preprocessing bottleneck.

4. [[1-Sparse Hamiltonian Decomposition via Graph Coloring]] — Decompose a sparse Hermitian matrix into a sum of 1-sparse Hermitian matrices (each row and column has at most one nonzero entry) by treating nonzero off-diagonal elements as graph edges and edge-coloring the resulting graph. Each 1-sparse matrix can be made unitary and self-inverse; the number of colors (terms) determines simulation overhead.

## References within this paper

| Ref | What it is | Vault note? |
|---|---|---|
| Babbush et al. (2016), NJP 18, 033032 | Previous paper by same group: Taylor series simulation of second-quantized chemistry Hamiltonians (both database and on-the-fly variants) | No (precursor to this paper) |
| Berry, Childs, Cleve, Kothari, Somma (2015), FOCS | Truncated Taylor series simulation — the LCU simulation technique this paper applies | No |
| Toloui & Love (2013) | Prior CI matrix coloring with $O(N^4)$ colors (this paper improves to $O(\eta^2 N^2)$) | No |
| Wecker, Hastings, Troyer (2015), PRA 92, 042303 | Tighter Trotter analysis for molecular simulation | No |
| Aspuru-Guzik et al. (2005), Science 309 | Original quantum simulation of chemistry proposal | No |
| Whitfield, Biamonte, Aspuru-Guzik (2011) | Second-quantized simulation using Trotter + Jordan-Wigner | No |
| Bravyi & Kitaev (2002) | Original BK fermion-to-qubit mapping | No |
| Seeley, Richard, Love (2012) | Analysis of Bravyi-Kitaev transformation | No |
| O'Malley, Babbush et al. (2016) | Hardware experiment; cites early version of this work | [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] |
| Childs (2010) | Sparse Hamiltonian simulation via quantum walk | No |

## Cross-links

### Paper notes
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Complementary qubitization approach by the same group; uses the dual basis and achieves T complexity $O(N^3/\varepsilon)$ with full surface-code resource estimates; better T-gate count than CI matrix approach, but worse qubit count for $\eta \ll N$
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — Hardware companion; cites an early version of this paper; uses the same Taylor series motivation but implements Trotter on hardware
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — Companion paper applying the same [[Truncated Taylor Series Simulation]] approach to real-space (position grid) first-quantized simulation; overlapping authors; complementary first-quantized strategy (grid vs. orbital basis)
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — Companion paper (same lead author, same year) using a second-quantized plane-wave dual basis instead of the CI first-quantized representation; achieves $\widetilde{O}(N^{8/3})$ circuit depth vs. this paper's $\widetilde{O}(\eta^2 N^3 t)$ gate count; targets periodic systems (materials) vs. this paper's molecular focus
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — Supersedes this paper for large $N$; uses first-quantized plane-wave encoding (momentum space) with interaction picture to achieve $\widetilde{O}(\eta^{8/3} N^{1/3} t)$, much better than $\widetilde{O}(\eta^2 N^3 t)$ whenever $N \gg \eta^{2/3}$
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Full constant-factor compilation of first-quantized plane-wave algorithms; renders the CI approach obsolete for large-basis molecular simulation; shows concrete resource estimates orders of magnitude better than second-quantized methods
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Second-quantized Trotter approach; provides practical near-term circuits (depth $N$, linear connectivity) that complement this paper's asymptotically better but more demanding LCU approach
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — Near-term variational approach; contrast with this paper's fault-tolerant focus; both address molecular simulation from opposite ends of the hardware maturity spectrum
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — applies a similar on-the-fly integral evaluation idea within DG blocks; complementary discretization approach to the CI matrix method
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — directly addresses the initial-state overlap problem for QPE; the CI matrix encoding here depends on having good overlap between the initial Slater determinant and the true ground state
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — later paper by same lead author; shifts from the CI first-quantized encoding to a real-space grid encoding and compares against classical mean-field (RT-TDHF/RT-TDDFT) rather than exact methods, establishing polynomial quantum advantage in dynamics
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — uses first-quantized plane-wave encoding (rather than CI orbital basis) with non-local pseudopotentials; represents the practical evolution of the first-quantized framework this paper introduced

### Trick cards
- [[CI Matrix Simulation (First-Quantized Encoding)]]
- [[Truncated Taylor Series Simulation]]
- [[On-the-fly Molecular Integral Evaluation]]
- [[1-Sparse Hamiltonian Decomposition via Graph Coloring]]
- [[Trotterized Time Evolution for Chemistry]] — what this paper supersedes for precision scaling
- [[Bravyi-Kitaev Transformation]] — alternative second-quantized encoding this paper avoids
- [[Iterative Phase Estimation (Kitaev)]] — the phase estimation wrapper needed to extract eigenvalues from the simulation
