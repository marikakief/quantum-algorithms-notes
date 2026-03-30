> **Source:** Casper Gyurik, Chris Cade, Vedran Dunjko, *Towards quantum advantage via topological data analysis*, arXiv:2005.02607, Quantum **6**, 855 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2005.02607) · [Quantum](https://quantum-journal.org/papers/q-2022-08-28-855/)
> **Tags:** #topological-data-analysis #Betti-numbers #quantum-advantage #DQC1 #dequantization #complexity-theory

---

## The computational problem

The paper studies several formally defined problems:

- **SUES (Sparse Uniform Eigenvalue Sampling):** Given sparse access to a PSD matrix $H$, sample from a distribution $(\delta, \mu)$-approximating the uniform distribution over eigenvalues.
- **LLSD (Low-Lying Spectral Density estimation):** Estimate the fraction of eigenvalues of $H$ below a threshold $b$, up to additive precision $\epsilon$.
- **ABNE (Approximate Betti Number Estimation):** Estimate the normalised approximate Betti number $N_{\Delta_k^G}(0, \delta)$ (fraction of eigenvalues of the combinatorial Laplacian below threshold $\delta$).
- **BNE (Betti Number Estimation):** Estimate the normalised Betti number $\beta_k^G / \dim H_k^G$.

The hierarchy is: LLSD generalises ABNE (for clique-dense graphs), and BNE reduces to ABNE when $\lambda_{\min} \geq 1/\text{poly}(n)$.

## What the paper does

Three things:

1. **DQC1-hardness of LLSD** (Theorem 1): Shows that estimating the low-lying spectral density of sparse PSD matrices is as hard as simulating the [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|one-clean-qubit model]]. This is the strongest evidence that quantum TDA resists dequantization — any classical algorithm for LLSD would imply efficient classical simulation of DQC1.

2. **DQC1-completeness for log-local Hamiltonians** (Theorem 2): When restricted to log-local Hamiltonians, LLSD is exactly DQC1-complete.

3. **New quantum algorithms** for rank estimation and spectral entropy estimation of combinatorial Laplacians, with applications to complex network analysis.

My assessment: this paper is the theoretical backbone of the quantum TDA advantage story. The DQC1-hardness result for LLSD eliminates generic dequantization attacks (à la Tang). The gap that remains — whether LLSD restricted to *combinatorial Laplacians of clique complexes* is still hard — is a structural question about whether clique complexes are "rich enough" to encode arbitrary computations. [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes|Gyurik et al. (2024)]] later proved BQP₁-hardness for a related persistence problem.

## The algorithm and hardness proof

### DQC1-hardness of LLSD

The proof reduces *normalised sub-trace estimation* (known to be DQC1-hard via Brandão) to LLSD:

1. Given a log-local Hamiltonian $H$ from Kitaev's circuit-to-Hamiltonian construction, construct a histogram of its low-lying eigenvalues using LLSD as a subroutine
2. Each "bin" count comes from the difference of two LLSD calls (upper and lower thresholds), yielding eigenvalue counts per bin accurate up to one-bin misplacement
3. The mean of this histogram approximates the normalised sub-trace
4. Since normalised sub-trace estimation is DQC1-hard, so is LLSD

The proof uses polynomial-time truth-table reductions (non-adaptive queries to LLSD oracle).

### LLSD solves ABNE for clique-dense graphs

For a clique-dense graph $G$ (satisfying $\binom{n}{k+1}/\chi_k \in O(\text{poly}(n))$), one can simulate sparse access to a padded matrix $\Gamma_k^G$ whose LLSD relates to the combinatorial Laplacian's LLSD via:

$$N_{\Delta_k^G}(0, b) = \frac{\binom{n}{k+1}}{\chi_k} N_{\Gamma_k^G}(0, b) - \frac{\binom{n}{k+1} - \chi_k}{\chi_k}$$

This shows LLSD is a legitimate generalisation of the problem the [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|LGZ algorithm]] efficiently solves.

### Quantum algorithms for rank estimation and spectral entropy

**Numerical rank estimation:** Since $r_H(b) = 1 - N_H(0,b)$, LLSD directly gives the numerical rank. The DQC1-hardness of LLSD implies an exponential quantum speedup for rank estimation of sparse matrices. For matrices with $\text{nnz}$ nonzero entries stored in qRAM, the quantum algorithm runs in $O(\text{poly}(n, \log \text{nnz}))$ vs. classical $O(\text{nnz})$.

**Spectral entropy estimation:** Constructs "purified quantum query access" to the eigenvalue distribution $p(\lambda_j) = \lambda_j / \text{Tr}(H)$ via QPE + controlled rotations + fixed-point [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]]. Sampling from this distribution enables spectral entropy estimation, useful for comparing complex networks.

## Key results

### Theorem 1 (DQC1-hardness)
LLSD is DQC1-hard, even for log-local Hamiltonians.

### Theorem 2 (DQC1-completeness)
LLSD restricted to log-local Hamiltonians is DQC1-complete.

### Speedup regimes

For clique sizes $k \geq 3$, graphs with edge density $\gamma > (k-2)/(2(k-1))$:
- The clique density theorem guarantees $\chi_k \in \Omega(n^{k+1})$ ("supersaturation")
- Quantum algorithm runs in $\tilde{O}(n^3)$
- Classical algorithm requires $O(n^{k+1})$
- For $k \sim \log n$: **superpolynomial** quantum speedup ($2^{O(\log n \log \log n)}$ vs. $2^{O((\log n)^2)}$)

### Connection to fermionic hardcore model

The combinatorial Laplacian of the complement graph $\bar{G}$ relates to the hardcore fermion Hamiltonian on $G$:

$$H_G = \bigoplus_{k=0}^{n-1} \Delta_k^{\bar{G}}$$

This links TDA complexity to the complexity of quantum many-body physics.

## Limits / caveats

1. **Gap between LLSD and ABNE.** DQC1-hardness of LLSD doesn't directly prove ABNE is classically hard, because ABNE restricts to combinatorial Laplacians. Whether this family is "rich enough" remains open.

2. **Approximate vs. exact Betti numbers.** The algorithm estimates approximate Betti numbers (eigenvalue count below a threshold), not exact Betti numbers. The distinction matters when $\lambda_{\min}$ is unknown.

3. **Practical utility of normalised Betti numbers.** The algorithm outputs $\beta_k / \dim H_k^G$, which is exponentially small in the interesting cases. Practical TDA applications need actual Betti numbers.

4. **Near-term prospects.** The paper discusses NISQ implementations but the spectral gap and state preparation requirements remain challenging.

## Reusable ideas

1. [[DQC1-Hardness via Histogram Approximation of Low-Lying Eigenvalues]] — Reduce normalised sub-trace estimation to spectral density estimation by binning eigenvalues. The key insight: misplacing eigenvalues by at most one bin still allows accurate mean computation.

2. [[Spectral Density Estimation as Dequantization Shield]] — If a quantum algorithm's core subroutine is LLSD (not just matrix inversion or SVD), it resists dequantization because LLSD is DQC1-hard for general sparse matrices. This is a meta-technique for arguing quantum advantage.

## References within this paper

- **[[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|Lloyd, Garnerone, Zanardi (2016)]]** — Original quantum TDA algorithm.
- **Brandão (2016)** — DQC1-hardness of normalised sub-trace estimation, which this paper's LLSD hardness proof builds on.
- **[[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|Knill-Laflamme (1998)]]** — DQC1 model.
- **Tang (2019), Tang et al.** — Dequantization results that motivated this investigation. This paper shows TDA is immune to those techniques.
- **Cade & Crichigno (2021)** — Showed DQC1-hardness for general chain complexes (not necessarily clique complexes). Cited as parallel work.
- **Alon & Shikhelman (2016)** — Clique density theorem (supersaturation).

## Cross-links

### Paper notes
- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]] — The algorithm whose advantage this paper establishes
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]] — Shows NP-hardness of Betti number approximation (complementary negative result)
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Improved algorithms + dequantization showing the advantage regime is narrow
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]] — BQP₁-hardness for persistence, closing the gap this paper identified
- [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes]] — Persistent Betti numbers via persistent Laplacians
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]] — DQC1 model

### Trick cards
- [[DQC1-Hardness via Histogram Approximation of Low-Lying Eigenvalues]] — New trick from this paper
- [[Spectral Density Estimation as Dequantization Shield]] — New trick from this paper
- [[Kernel Dimension Estimation via QPE on Mixed States]] — Core QPE technique used
- [[Simplex-to-Qubit Encoding for Simplicial Complexes]] — Encoding inherited from LGZ
