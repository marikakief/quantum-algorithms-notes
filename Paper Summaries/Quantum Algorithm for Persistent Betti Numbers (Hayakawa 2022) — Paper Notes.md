> **Source:** Ryu Hayakawa, *Quantum algorithm for persistent Betti numbers and topological data analysis*, arXiv:2111.00433, Quantum **6**, 873 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2111.00433) · [Quantum](https://quantum-journal.org/papers/q-2022-12-07-873/)
> **Tags:** #topological-data-analysis #persistent-Betti-numbers #persistent-Laplacian #QSVT #block-encoding #Schur-complement

---

## The computational problem

Given a **simplicial pair** $K \hookrightarrow L$ (simplicial complexes with $K \subseteq L$ over $n$ vertices), estimate the $q$-th **persistent Betti number** $\beta_q^{K,L}$, which counts the number of $q$-dimensional holes present in $K$ that are still alive in $L$.

This is strictly more general than ordinary Betti numbers: $\beta_q^{K,K} = \beta_q^K$ (the usual Betti number). Persistent Betti numbers are the more useful TDA invariant because they distinguish "real" topological features (long-lived holes) from noise (short-lived holes).

Classical algorithms require $O((n_q^L)^\omega)$ time ($\omega < 2.373$), where $n_q^L$ is the number of $q$-simplices in $L$ — exponential in $n$ for high-dimensional simplices.

## What the paper does

Gives the **first quantum algorithm for persistent Betti numbers** of arbitrary dimension. The key insight is that persistent Betti numbers equal the nullity of the **persistent Laplacian** $\Delta_q^{K,L}$ (Theorem 1, from Wang-Nguyen-Wei 2020), and this persistent Laplacian can be block-encoded using a Schur complement construction.

The algorithm uses [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] instead of QPE, achieving $\log(1/\epsilon)$ scaling in precision (though this doesn't improve the overall sampling-dominated complexity).

My assessment: this fills the most important gap in the quantum TDA programme. The [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|LGZ algorithm]] could only estimate ordinary Betti numbers at each scale separately, not track which holes persist across scales. With persistent Betti numbers, one can recover persistent barcodes — the standard output of TDA. The construction is technically clean, using Schur complements and block-measurement rather than ad hoc methods. The practical limitation remains the same: normalised persistent Betti numbers, clique-density requirement, spectral gap assumption.

## The algorithm

### The persistent Laplacian

For $K \hookrightarrow L$, the $q$-th persistent Laplacian is:

$$\Delta_q^{K,L} = \underbrace{\partial_{q+1}^{L,K} (\partial_{q+1}^{L,K})^*}_{\Delta_{q,\text{up}}^{K,L}} + \underbrace{(\partial_q^K)^* \partial_q^K}_{\Delta_{q,\text{down}}^K}$$

where $\partial_{q+1}^{L,K}$ is the boundary operator of $L$ restricted so that its image lies in $C_q^K$ (the chain space of $K$). The "up" part involves simplices in $L$ but not in $K$; the "down" part is the usual $K$-Laplacian.

**Theorem 1 (Wang-Nguyen-Wei, Memoli-Wan-Wang):** $\beta_q^{K,L} = \text{nullity}(\Delta_q^{K,L})$.

### Block-encoding via Schur complement

The up-Laplacian $\Delta_{q,\text{up}}^{K,L}$ is computed via:

$$\Delta_{q,\text{up}}^{K,L} = \Delta_{q,\text{up}}^L / \Delta_{q,\text{up}}^L(I_K^L, I_K^L)$$

where $I_K^L = [n_q^L] \setminus [n_q^K]$ indexes the simplices in $L$ but not in $K$, and $/$ denotes the Schur complement. This avoids explicitly constructing the persistent Laplacian's matrix representation.

The Schur complement is implemented quantum-mechanically using the **pseudo-inverse** via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] polynomial approximation of $1/x$.

### Full algorithm pipeline

1. **State preparation:** Prepare $|\phi_q^K\rangle = \frac{1}{\sqrt{n_q^K}} \sum_i |\sigma_q^K(i)\rangle$ (uniform superposition over $q$-simplices of $K$) using [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] + [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes|fixed-point amplitude amplification]].

2. **Block-encode projector $\Pi$:** Construct an approximate block-encoding $\tilde{U}_\Pi$ of the projector onto $\ker \Delta_q^{K,L}$ using:
   - Block-encode the persistent Laplacian (boundary operators + Schur complement + pseudo-inverse)
   - Apply QSVT with a rectangle function polynomial to project onto the zero eigenspace

3. **Block-measurement:** Use the block-measurement technique (Rall 2020) to implement a quantum channel approximating $|0\rangle \otimes |\psi\rangle \to |1\rangle \otimes \Pi|\psi\rangle + |0\rangle \otimes (I-\Pi)|\psi\rangle$

4. **Estimate:** The probability of measuring $|1\rangle$ in the first qubit is $\approx \beta_q^{K,L} / n_q^K$ (normalised persistent Betti number).

## Key results

### Theorem 7 (Main result)

Under promises:
- (P1) $K$ is $q$-simplex dense: $d_q^K = n_q^K / \binom{n}{q+1} \in \Omega(1/\text{poly}(n))$
- (P2) $\Delta_{q,\text{up}}^L(I_K^L, I_K^L)$ has inverse-polynomial spectral gap $\gamma_{\min}^q$
- (P3) The persistent Laplacian $\Delta_q^{K,L}$ has inverse-polynomial spectral gap $\lambda_{\min}^q$

the algorithm estimates $\beta_q^{K,L}/n_q^K$ to additive error $\epsilon$ using:
- $O(n) + O(\log n)$ qubits
- $\tilde{O}(\text{poly}(n) \cdot (\log 1/\epsilon)^2)$ oracle calls and gates

### Complexity breakdown (without promises)

$$O_q^K: O\left(\sqrt{1/d_q^K} \cdot \log(1/\epsilon_{\text{sign}})\right) + O\left(\frac{q^4 n^6}{(\gamma_{\min}^q)^2 \lambda_{\min}^q} \cdot \log(1/\epsilon_{\text{rect}}) \cdot \log(1/\gamma_{\min}^q \epsilon_{\text{inv}})\right)$$

### Improvement over LGZ

The QSVT approach gives $O(\log(1/\epsilon))$ precision scaling in the block-encoding step (vs. $O(1/\delta)$ for QPE in LGZ). However, the overall complexity is dominated by the $O(1/\epsilon^2)$ sampling repetitions, so this improvement doesn't change the asymptotic scaling.

## Comparison with prior work

| Feature | LGZ (2016) | This paper |
|---|---|---|
| Computes | Ordinary Betti numbers $\beta_q^K$ | **Persistent** Betti numbers $\beta_q^{K,L}$ |
| Technique | QPE + Hamiltonian simulation | QSVT + block-encoding |
| Precision scaling (per call) | $O(1/\delta)$ | $O(\log(1/\epsilon))$ |
| Handles persistence? | Only via separate runs at each scale | **Yes**, directly via persistent Laplacian |
| Spectral gap assumptions | $\lambda_{\min}$ of $\Delta_q^K$ | $\lambda_{\min}$ of $\Delta_q^{K,L}$ AND $\gamma_{\min}$ of Schur complement |

## Limits / caveats

1. **Still normalised.** Estimates $\beta_q^{K,L}/n_q^K$, not $\beta_q^{K,L}$ itself. Same exponential-smallness issue as LGZ.

2. **Two spectral gap assumptions.** Requires inverse-polynomial gaps for both the persistent Laplacian and the Schur complement submatrix. The second assumption (P2) is new and less understood.

3. **Practical utility unclear.** Whether normalised persistent Betti numbers are useful for real TDA applications remains open. Standard TDA output (barcodes) requires actual persistent Betti numbers.

4. **Higher polynomial overhead.** The dependence on $q^4 n^6 / (\gamma_{\min}^q)^2$ is steeper than LGZ's $n^3$ for ordinary Betti numbers, due to the Schur complement and pseudo-inverse constructions.

## Reusable ideas

1. [[Persistent Laplacian for Tracking Topological Persistence]] — The persistent Laplacian $\Delta_q^{K,L}$ has nullity equal to the persistent Betti number. This operator-theoretic characterisation of persistence (from Wang-Nguyen-Wei 2020) is the key enabler: it converts a topological question into a spectral question.

2. [[Schur Complement Block-Encoding for Projected Operators]] — Block-encode a Schur complement $M/D = A - BD^+C$ by composing block-encodings of $A$, $B$, $C$, and the pseudo-inverse $D^+$ (via QSVT). Useful whenever a quantum operator is defined via projection onto a subspace.

## References within this paper

- **[[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|Lloyd, Garnerone, Zanardi (2016)]]** — Original quantum TDA algorithm; this paper extends it to persistence.
- **Wang, Nguyen, Wei (2020); Memoli, Wan, Wang (2022)** — Persistent Laplacian theory: $\beta_q^{K,L} = \text{nullity}(\Delta_q^{K,L})$.
- **[[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low, Wiebe (2019)]]** — QSVT framework used for polynomial transformations.
- **Rall (2020)** — Block-measurement technique.
- **[[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes|Yoder, Low, Chuang (2014)]]** — Fixed-point amplitude amplification for state preparation.
- **[[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes|Gyurik, Cade, Dunjko (2022)]]** — DQC1-hardness evidence for the TDA problem.

## Cross-links

### Paper notes
- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]] — Original algorithm this paper extends
- [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]] — Complexity-theoretic evidence for quantum advantage
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Further improvements and dequantization
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]] — BQP₁-hardness for harmonic persistence (by same author)
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]] — NP-hardness of Betti number approximation
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — Framework used for block-encoding

### Trick cards
- [[Persistent Laplacian for Tracking Topological Persistence]] — New trick from this paper
- [[Schur Complement Block-Encoding for Projected Operators]] — New trick from this paper
- [[Kernel Dimension Estimation via QPE on Mixed States]] — Related technique (QPE version)
- [[Simplex-to-Qubit Encoding for Simplicial Complexes]] — Encoding used
