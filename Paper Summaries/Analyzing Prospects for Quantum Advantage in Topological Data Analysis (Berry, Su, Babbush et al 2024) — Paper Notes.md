> **Source:** Dominic W. Berry, Yuan Su, Casper Gyurik, Robbie King, Joao Basso, Alexander Del Toro Barba, Abhishek Rajput, Nathan Wiebe, Vedran Dunjko, and Ryan Babbush, *Analyzing Prospects for Quantum Advantage in Topological Data Analysis*, arXiv:2209.13581, PRX Quantum **5**, 010319 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2209.13581) · [PRX Quantum](https://journals.aps.org/prxquantum/abstract/10.1103/PRXQuantum.5.010319)
> **Tags:** #topological-data-analysis #Betti-numbers #quantum-advantage #dequantization #fault-tolerant #resource-estimation #amplitude-estimation #qubitization #combinatorial-Laplacian

---

## The computational problem

Given an undirected graph $G = (V, E)$ with $n$ vertices, form its **clique complex** — the abstract simplicial complex where every $k$-clique of $G$ becomes a $(k-1)$-simplex. The $(k-1)$-th **Betti number** $\beta_{k-1}^G$ counts the number of independent $k$-dimensional "holes" in this complex and equals the dimension of the kernel of the **combinatorial Laplacian**:

$$\Delta_{k-1}^G = \partial_{k-1}^{G\dagger} \partial_{k-1}^G + \partial_k^G \partial_k^{G\dagger}$$

where $\partial_k^G : \mathcal{H}_k^G \to \mathcal{H}_{k-1}^G$ is the boundary map restricted to the clique subspace. The space $\mathcal{H}_k^G$ is spanned by basis states corresponding to $k$-cliques and has dimension $|\text{Cl}_k(G)|$.

**Input:** A classical database of edges (or missing edges) of $G$.

**Output:** An estimate of $\beta_{k-1}^G$ to relative error $r$ with failure probability $\delta$.

Classical algorithms scale at best as $\Omega(|\text{Cl}_k(G)|)$, which can be as large as $\binom{n}{k}$ — exponential in $k$. The question is whether quantum algorithms can do substantially better.

## What the paper does

Three things at once:

1. **Optimized quantum algorithm** for Betti number estimation with full fault-tolerant compilation and constant factors. Improves over Lloyd et al. (2016), Gunn & Kornerup (2019), and Gyurik, Cade & Dunjko (2022) by ~3–4 orders of magnitude in Toffoli count.

2. **Careful analysis of when quantum advantage exists.** Super-quadratic speedup requires targeting *multiplicative* error and having asymptotically large Betti number. Constructs specific graph families ($K(m,k)$ complete multipartite, Erdős–Rényi, Rips complexes) and characterizes their speedup regimes.

3. **Partial dequantization.** Introduces a randomized classical algorithm using path-integral Monte Carlo that can estimate normalized Betti numbers in polynomial time for clique-dense graphs under certain conditions on the Markov chain gap. This shows that exponential dimension alone is *not* sufficient for quantum advantage — a subtlety previous work missed.

My assessment: this is one of the most thorough quantum advantage analyses for any problem outside chemistry. The conclusion is nuanced and honest — superpolynomial speedups exist but require very specific parameter regimes, and even then the best classical algorithms (particularly the Apers et al. algorithm published shortly after) close some of the gap. The paper is also unusually careful about constant factors, which is where most quantum advantage claims die.

## The algorithm

The algorithm estimates $\beta_{k-1}^G$ by measuring the dimension of the kernel of the Dirac operator $B_G$, which block-encodes the boundary maps:

$$B_G = \begin{pmatrix} 0 & \partial_{k-1}^G & 0 \\ \partial_{k-1}^{G\dagger} & 0 & \partial_k^G \\ 0 & \partial_k^{G\dagger} & 0 \end{pmatrix}$$

The zero eigenspace of $B_G$ on $\mathcal{H}_k^G$ has dimension $\beta_{k-1}^G$. The algorithm pipeline:

### Step 1: Prepare a uniform mixture over $k$-cliques

Start with a Dicke state (uniform superposition over all Hamming-weight-$k$ basis states), then project onto the clique subspace using amplitude amplification.

- **[[Dicke State Preparation via Inequality Testing|Dicke state preparation]]**: Two new schemes. The first uses inequality testing on $n$ registers in superposition to find a threshold giving exactly $k$ ones — cost $\tilde{O}(n)$ Toffolis. The second prepares $k$ random positions and checks for collisions — cost $(k+2)n + O(k\log n)$ Toffolis, success probability $k!/n^k \cdot \binom{n}{k}$. Both allow entanglement with garbage, which is fine for the application.

- **Clique detection**: Given a Hamming-weight-$k$ string, check if it represents a $k$-clique by counting edges among the selected vertices. Cost: $3|E| + 2\log k$ Toffolis (edge database) or $2|E^C|$ Toffolis (missing-edge database). Uses the bit-summing technique from [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders et al. 2020]].

- **[[Kaiser Window Amplitude Estimation|Amplitude estimation with Kaiser windows]]**: Estimate the clique fraction $\sqrt{|\text{Cl}_k(G)|/\binom{n}{k}}$ once, then use standard amplitude amplification (with the estimated number of steps) many times. This separation avoids the $\log$ overhead of fixed-point methods. The Kaiser window technique itself achieves $N = \frac{\pi}{\epsilon}\sqrt{1 + \alpha^2}$ oracle calls for error $\epsilon$ with failure probability $\delta$, where $\alpha$ depends on $\delta$ — about an order of magnitude better than Brassard, Høyer & Tapp (1998).

### Step 2: Block-encode the Dirac operator

The unrestricted Dirac operator has the Jordan-Wigner form:

$$B = \sum_{j=1}^n Z_1 \otimes \cdots \otimes Z_{j-1} \otimes X_j$$

Block-encode $B_G/\lambda$ (with $\lambda \approx n$) by preparing an equal superposition over $n$ terms, applying the controlled Pauli string, and projecting onto the clique subspace with correct Hamming weight. The [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] walk operator is then $W = RV$ where $R = i(2|0\rangle\langle 0| \otimes P - I)$ includes the projection $P$ onto valid states.

Cost per walk step: $6|E| + 5n + O(\log n)$ Toffolis (edge database) or $4|E^C| + 5n + O(\log n)$ (missing edges).

This is a significant improvement over prior work that simulated $e^{iB_G t}$ via Hamiltonian simulation — [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] avoids the logarithmic overhead from short-time simulation needed to prevent eigenvalue wraparound.

### Step 3: Project onto the zero eigenspace

Use a **Chebyshev polynomial filter** on the walk operator to suppress all non-zero eigenvalues by a factor $\epsilon$. Zero eigenvalues of $B_G$ map to eigenvalues $\pm 1$ of the walk operator. The filter has degree:

$$\ell = \frac{\cosh^{-1}(1/\epsilon)}{\cosh^{-1}(1/\sqrt{1 - (\lambda_{\min}/\lambda)^2})} \leq \frac{n}{\lambda_{\min}} \ln(2/\epsilon)$$

where $\lambda_{\min}$ is the spectral gap of $\Delta_{k-1}^G$. This is asymptotically similar to phase estimation but avoids the overhead of producing a full eigenvalue estimate when you only need a binary decision (zero vs. non-zero).

### Step 4: Amplitude estimation on the filtered state

Use [[Kaiser Window Amplitude Estimation|Kaiser-window amplitude estimation]] again to estimate the overlap of the filtered state with the zero eigenspace, giving $\sqrt{\beta_{k-1}^G / |\text{Cl}_k(G)|}$.

## Key results

### Theorem (Total complexity, Lemma 1)

The Toffoli cost of estimating $\beta_{k-1}^G$ to relative error $r$ with failure probability $\delta$ is:

$$T(G,k,r,\delta) = \frac{6|E|\ln(1/\delta)}{r} \sqrt{\frac{|\text{Cl}_k(G)|}{\beta_{k-1}^G}} \left[ \frac{\pi}{2}\sqrt{\frac{\binom{n}{k}}{|\text{Cl}_k(G)|}} + \frac{n}{\lambda_{\min}} \ln\!\left(\frac{4|\text{Cl}_k(G)|}{r\,\beta_{k-1}^G}\right) \right]$$

with $6|E|$ replaced by $4|E^C|$ when using missing-edge database.

### Simplified scaling

Dropping log factors:

$$T_q = \tilde{O}\!\left(\frac{n|E|}{r\,\lambda_{\min}} \sqrt{\frac{1}{\beta_{k-1}} \binom{n}{k}}\right)$$

Classical lower bound: $T_c = \Omega(|\text{Cl}_k(G)|)$.

When $\beta_{k-1} = O(1)$: only **quadratic** speedup over classical.

When $\beta_{k-1}$ is large (scaling with $\binom{n}{k}$): can achieve **super-quadratic** speedup for relative error.

### Concrete resource estimate

For $K(16,16)$ (complete 16-partite graph, $n = 256$, $k = 16$): **~6.8 billion Toffolis**. This is comparable to classically intractable chemistry instances (cf. [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. 2021]]). The classical cost is $\sim 2 \times 10^{19}$ (number of cliques) to $\sim 10^{25}$ ($\binom{256}{16}$).

## Comparison with prior work

| Feature | Lloyd et al. 2016 | Gunn & Kornerup 2019; Gyurik et al. 2022 | **This paper** |
|---|---|---|---|
| Dicke state prep | Superposition over all $k$ | $\tilde{O}(k^2 n \log^2 n)$ Toffolis | $\tilde{O}(n)$ or $(k+2)n$ Toffolis |
| Clique detection | $O(k^2)$ oracle calls | Same as Lloyd | $3\|E\| + O(\log k)$ or $2\|E^C\|$ Toffolis |
| Dirac operator | Hamiltonian simulation | Hamiltonian simulation | [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]] (walk operator) |
| Kernel projection | Phase estimation | Phase estimation | [[Dolph-Chebyshev Eigenstate Filtering|Chebyshev filter]] |
| Final estimation | Classical sampling | Amplitude estimation | [[Kaiser Window Amplitude Estimation|Kaiser-window amplitude estimation]] |
| Constant factors | Not computed | Not computed | **Fully compiled** |
| Net improvement | — | — | **~3–4 orders of magnitude** |

## Speedup regimes

### Complete multipartite graphs $K(m,k)$

$k$ clusters of $m$ vertices, all inter-cluster edges present:
- $\beta_{k-1} = (m-1)^k$, $\lambda_{\min} = m$, $|\text{Cl}_k| = m^k$
- Constant $m$: polynomial speedup by $2\ln m$ root
- Constant $k$: polynomial speedup by $k/2$ root
- $k = c\ln^2 n$: **superpolynomial** speedup by $\sim 2\ln n$ root (not exponential — data input bottleneck)

### Erdős–Rényi $G(n,p)$

At $p = n^{-1/(k+1/2)}$, the quantum algorithm achieves roughly a **quartic speedup** for constant $k$. This applies to generic random graphs — not specially constructed.

### Rips complexes from $\mathbb{R}^d$ data

For data drawn from a probability measure on $\mathbb{R}^d$: Betti numbers grow at most linearly in $n$ (Kahle 2011), far from the $n^k$ needed for superpolynomial speedup. However, artificial point configurations in $\mathbb{R}^2$ can achieve $(m-1)^k$ Betti numbers via a rotated dipole construction.

## Limits / caveats

1. **Additive vs. multiplicative error matters enormously.** For additive error in $\beta_{k-1}$, quantum algorithms always have exponential runtime (the normalization factor $|\text{Cl}_k(G)|$ blows up). The super-quadratic advantage only exists for *relative* error estimation when the Betti number is large.

2. **The dequantization.** A randomized classical algorithm using path-integral Monte Carlo on the squared Dirac operator can estimate normalized Betti numbers in $\tilde{O}(\sigma^2/\epsilon^2 \cdot \text{poly}(\kappa, 1/\gamma_M, |E|))$ arithmetic operations — polynomial in $n$ when $\sigma$, $\kappa$, $\gamma_M^{-1}$ are at most $\text{poly}(n)$. This directly challenges the assumption that clique-dense cases guarantee quantum advantage. The catch: the Markov chain gap $\gamma_M$ is unknown in general, making it hard to evaluate.

3. **Apers et al. (2022) further close the gap.** Their classical PIMC algorithm runs in $n^{O(\gamma^{-1}\log(1/\epsilon))}$ for general complexes. For constant normalized spectral gap and constant error, it's polynomial. But it breaks down for inverse-polynomial error or small spectral gap — exactly where the quantum algorithm still works.

4. **Real-world TDA data doesn't have the right structure.** Rips complexes from finite-dimensional data have Betti numbers growing at most linearly. The superpolynomial advantage requires abstract graphs with exponentially large Betti numbers — these exist but are constructed specifically to have this property.

5. **Exact Betti number computation is QMA$_1$-hard** (Crichigno & Kohler 2022), which is likely beyond even quantum computers. This paper works with approximate/relative-error estimation, not exact computation.

6. **Complexity-theoretic status is incomplete.** Estimating normalized Betti numbers to additive precision $1/\text{poly}(n)$ is DQC1-hard for general Hermitian operators. Whether this hardness persists when restricted to combinatorial Laplacians of clique complexes remains open.

## Reusable ideas

1. [[Kaiser Window Amplitude Estimation]] — Improved amplitude estimation using Kaiser window functions; achieves ~10× fewer oracle calls than Brassard, Høyer & Tapp at practical failure probabilities. Separating estimation (once) from amplification (many times) avoids the log overhead of fixed-point methods.

2. [[Dicke State Preparation via Inequality Testing]] — Two efficient schemes for preparing Dicke states (uniform superpositions of fixed Hamming weight) allowing garbage entanglement. The threshold scheme costs $\tilde{O}(n)$ Toffolis; the collision-based scheme costs $(k+2)n$ Toffolis with success probability from the birthday problem.

3. [[Chebyshev Kernel Projection for Walk Operators]] — When you only need to distinguish zero from nonzero eigenvalues (rather than estimate eigenvalues), a Chebyshev polynomial filter on the qubitized walk operator is more efficient than phase estimation. Cost: $(n/\lambda_{\min})\ln(2/\epsilon)$ walk steps.

4. [[Clique Checking via Edge Counting]] — Verify that an $n$-qubit computational basis state represents a $k$-clique by summing edge-presence bits. Cost: $3|E| + 2\log k$ Toffolis using the efficient bit-sum from Sanders et al. 2020, or $2|E^C|$ using missing-edge database with multiply-controlled Toffolis.

## References within this paper

- **Lloyd, Garnerone, Zanardi (2016)** — Original quantum TDA algorithm. Nature Communications 7, 10138. Proposed amplitude amplification + QPE on the Dirac operator for Betti number estimation.
- **Gunn & Kornerup (2019)** — arXiv:1906.07673. Corrected some of Lloyd et al.'s scaling claims; showed quadratic speedup under certain assumptions.
- **Gyurik, Cade & Dunjko (2022)** — *Towards quantum advantage via topological data analysis*, Quantum 6, 855. Showed DQC1-hardness for normalized low-lying spectral density estimation. [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders et al. 2020]] is cited for efficient bit-summing.
- **Cade & Crichigno (2021)** — arXiv:2107.00011. Fermionic representation of the Dirac operator; DQC1-hardness for general chain complexes.
- **Crichigno & Kohler (2022)** — arXiv:2209.11793. QMA$_1$-hardness of clique homology (deciding $\beta_k = 0$ vs $\beta_k > 0$).
- **Ubaru et al. (2021)** — arXiv:2108.02811. QSVT-based kernel projector for TDA with linear depth.
- **Hayakawa (2022)** — Quantum 6, 873. Quantum algorithm for persistent Betti numbers.
- **Apers, Sen & Szabó (2022)** — arXiv:2211.09618. Classical PIMC algorithm for normalized Betti numbers; polynomial for constant gap and constant error.
- **Low & Chuang (2019)** — [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]]. Quantum 3, 163. Walk operator construction used here.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Controlled Pauli string technique (Figure 9) used for the Dirac operator implementation; also source of [[Unary Iteration|unary iteration]].
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders et al. (2020)]] — Efficient bit-summing procedure used in clique checking; also source of [[QROM (Quantum Read-Only Memory)|QROM]] equal-superposition preparation.
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al. (2020)]] — Single-Toffoli-per-addition bit-summing method.
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. (2022)]] — Related quantum advantage analysis for chemistry.
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes|Babbush, Berry, Kothari, Somma, Wiebe (2023)]] — Another setting where the quantum advantage question is carefully analyzed.

## Cross-links

### Paper notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — Bit-summing, equal-superposition preparation, and the broader "focus beyond quadratic speedups" theme
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Controlled Pauli string technique, [[Unary Iteration|unary iteration]], [[QROM (Quantum Read-Only Memory)|QROM]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — Efficient bit-summing
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — Comparable Toffoli count (~$5 \times 10^9$) for a different classically intractable problem
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — Parallel dequantization/advantage analysis in chemistry
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — Another quantum advantage analysis with honest assessment of speedup regimes
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]] — Achieves the quartic speedup regime this paper identifies as promising; different problem (planted inference vs. TDA) but same "focus beyond quadratic speedups" theme
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — Cited for qubitization complexity estimates
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — same cluster of Babbush-group "when does quantum advantage actually exist?" papers; both use dequantization arguments to sharpen the case for/against advantage

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — Core walk operator construction
- [[Dolph-Chebyshev Eigenstate Filtering]] — Related filtering technique; this paper uses a Chebyshev filter on the walk operator rather than the QLSP-specific version
- [[Unary Iteration]] — Used in controlled operations
- [[QROM (Quantum Read-Only Memory)]] — Used for equal-superposition state preparation
- [[Kaiser Window Amplitude Estimation]] — New trick from this paper
- [[Dicke State Preparation via Inequality Testing]] — New trick from this paper
- [[Chebyshev Kernel Projection for Walk Operators]] — New trick from this paper
- [[Clique Checking via Edge Counting]] — New trick from this paper
