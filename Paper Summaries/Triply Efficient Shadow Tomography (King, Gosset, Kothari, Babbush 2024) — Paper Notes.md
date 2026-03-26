> **Source:** Robbie King, David Gosset, Robin Kothari, Ryan Babbush, *Triply Efficient Shadow Tomography*, arXiv:2404.19211, PRX Quantum **6**, 010336 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2404.19211) · [Journal](https://doi.org/10.1103/PRXQuantum.6.010336)
> **Tags:** #shadow-tomography #classical-shadows #Pauli-learning #fermionic #graph-coloring #chi-boundedness #Bell-sampling #two-copy #measurement #RDM

---

## The computational problem

**Pauli shadow tomography.** Given copies of an unknown $n$-qubit state $\rho$ and a set $S \subseteq \mathcal{P}(n)$ of Pauli operators, output estimates $y_P$ such that $|y_P - \mathrm{tr}(P\rho)| \leq \epsilon$ for all $P \in S$, with high probability.

The paper asks: when can this be done **triply efficiently**?

1. **Sample efficiency:** number of copies scales as $\mathrm{poly}(\log|S|, 1/\epsilon)$
2. **Computational efficiency:** total runtime $\mathrm{poly}(|S|, n, 1/\epsilon)$
3. **Few-copy measurements:** only joint measurements on $O(1)$ copies of $\rho$ at a time (ideally 1 or 2)

The three observable sets of interest:

| Set | Size | Description |
|---|---|---|
| $\mathcal{P}^{(n)}_k$ | $3^k \binom{n}{k}$ | $k$-local Pauli operators |
| $\mathcal{F}^{(n)}_k$ | $\binom{2n}{2k}$ | $k$-body fermionic operators (Majorana monomials of degree $2k$) |
| $\mathcal{P}(n)$ | $4^n$ | All $n$-qubit Paulis |

## What the paper does

Two-copy measurements are both necessary and sufficient for sample-efficient shadow tomography of local fermionic operators and the full Pauli set. This is the central message.

For $k$-local Paulis, single-copy protocols (random Pauli basis measurements, classical shadows) already achieve triple efficiency. But for $k$-body fermionic operators and for all Paulis, single-copy measurements provably cannot be sample-efficient. The paper closes the gap by showing two-copy Clifford measurements suffice.

The framework is surprisingly clean: an initial round of [[Bell Difference Sampling for Magnitude Estimation|Bell sampling]] learns all magnitudes $|\mathrm{tr}(\rho P)|$, reducing the sign-learning problem to a graph coloring question on the induced subgraph of large-magnitude Paulis. The uncertainty principle bounds the clique number of this subgraph, and chi-boundedness results from graph theory then yield good fractional colorings.

My assessment: this is an excellent paper. The connection between shadow tomography and graph-theoretic chi-boundedness is the kind of structural insight that opens a research direction rather than closing one. The proof that two copies are necessary and sufficient — with single copies provably failing for fermions — is a clean separation that clarifies the landscape. The practical implications for fermionic RDM measurement ($k=1$: $O((\log n)/\epsilon^4)$ two-copy measurements) are immediate and concrete.

## The algorithm / construction

### Stage 1: Learn magnitudes via Bell sampling

Measure copies of $\rho \otimes \rho$ in the Bell basis — the Clifford basis that simultaneously diagonalizes $P \otimes P$ for all $P \in \mathcal{P}(n)$. Since $\mathrm{tr}((P \otimes P)(\rho \otimes \rho)) = \mathrm{tr}(\rho P)^2$, this gives estimates of $|\mathrm{tr}(\rho P)|$ using $O((\log|S|)/\epsilon^4)$ two-copy measurements.

Define the "significant" set:
$$S_\epsilon = \{P \in S : |u_P| \geq 3\epsilon/4\}$$
where $u_P$ are the magnitude estimates. With high probability, $|\mathrm{tr}(\rho P)| \geq \epsilon/2$ for all $P \in S_\epsilon$.

### Stage 2: Learn signs

The sign-learning task reduces to a coloring problem on the commutation graph $G(S_\epsilon)$.

**Key structural insight (Lemma 8):** the clique number of $G(S_\epsilon)$ is at most $4/\epsilon^2$, with high probability. This follows from the uncertainty principle — pairwise anticommuting Paulis satisfy $\sum_j \mathrm{tr}(P_j \rho)^2 \leq 1$, and every Pauli in $S_\epsilon$ has $|\mathrm{tr}(\rho P)| \geq \epsilon/2$.

Once the clique number is bounded, the paper pursues two routes:

**Route A (all Paulis — Theorem 7):** Compute a "[[Mimicking State via Matrix Multiplicative Weights|mimicking state]]" $\sigma$ satisfying $|\mathrm{tr}(\sigma P)| \geq \epsilon/4$ for all $P \in S_\epsilon$, using the matrix multiplicative weights algorithm. Then do Bell sampling on $\rho \otimes \sigma$: the product $\mathrm{tr}(P\rho)\mathrm{tr}(P\sigma)$ has known sign (since $\sigma$ is known), revealing the sign of $\mathrm{tr}(P\rho)$. Sample complexity: $O(n\log(n/\epsilon)/\epsilon^4)$. Runtime: $\mathrm{poly}(2^n, 1/\epsilon)$ — exponential, but that's fine for learning all $4^n$ Paulis.

**Route B (fermionic operators — Theorem 10):** Use [[Fractional Coloring of Commutation Graphs|fractional graph coloring]] on $G(S_\epsilon)$. The commutation graph has bounded clique number (Lemma 8), and the family of induced subgraphs of $G(\mathcal{F}^{(n)}_k)$ is chi-bounded (Lemma 9). The fractional coloring yields commuting Pauli groups that can be measured with single-copy Clifford measurements. Combined with the initial Bell sampling stage, this gives a triply efficient two-copy protocol.

### The coloring machinery in detail

**Single-copy learning via fractional coloring (Theorem 5):** Given a fractional coloring of $G(S)$ of size $\chi$, there is a single-copy Clifford learning algorithm using $O(\chi(\log|S|)/\epsilon^2)$ copies. Each sample: draw an independent set $I$ from the fractional coloring, measure $\rho$ in the Clifford basis that diagonalizes all Paulis in $I$. Process: median-of-means estimator.

**Two-copy reduction (Lemma 20):** Bell sampling stage costs $O((\log|S|)/\epsilon^4)$ two-copy measurements. Then single-copy learning on $S_\epsilon$ costs $O(\chi(\log|S|)/\epsilon^2)$ single-copy measurements. Total: $O((\log|S|)/\epsilon^4 + \chi(\log|S|)/\epsilon^2)$.

**Chi-binding for fermionic operators (Lemma 9):** The fractional chromatic number of any induced subgraph of $G(\mathcal{F}^{(n)}_k)$ with clique number $\omega$ satisfies $\chi_f \leq p_k(\omega)$ where $p_k$ is a polynomial independent of $n$. The proof is by induction on the Majorana monomial degree:

- **Base case ($k=1$):** The commutation graph of 1-body fermionic operators $i c_a c_b$ is the line graph of an auxiliary graph on Majorana modes. Edge coloring via Misra-Gries gives $\chi \leq \omega + 1$.
- **Inductive step (odd degree):** Find a maximal anticommuting set (size $\leq \omega$), partition the vertex set by first Majorana index in the support of that set, reduce each partition element to degree $r-1$.
- **Inductive step (even degree):** For each Majorana index $i$, collect operators containing $c_i$, strip it off to reduce to degree $r-1$. Sample independent sets from each sub-problem and intersect.

The resulting polynomials satisfy $p_1(\omega) = \omega+1$, $p_2(\omega) = O(\omega^8)$, and in general $p_k(\omega) \leq (2k\omega)^{(2k)^{k+1}}$.

**Chi-binding for all Paulis (Lemma 11):** The commutation graph $G(\mathcal{P}(n))$ has no induced paths with more than $2n+1$ vertices. (The prefix products $Q_r = P_1 P_2 \cdots P_r$ of an induced path are pairwise anticommuting, and $\leq 2n+1$ pairwise anticommuting Paulis exist.) Then Gyárfás's theorem gives $\chi(G') \leq (2n+1)^{\omega-1}$.

### Neighbour-first search

To algorithmically exploit chi-boundedness, the paper introduces a graph traversal algorithm — "neighbour-first search" (NFS) — that produces a spanning tree whose levels have clique number one less than the original graph. This enables an inductive coloring: color each level recursively, using disjoint color sets.

## Key results

**Theorem 6.** For any $S \subseteq \mathcal{P}(n)$, there exists a shadow tomography protocol using only $O((\log|S|)/\epsilon^4)$ two-copy Clifford measurements.

**Theorem 7 (All Paulis, triply efficient).** Shadow tomography for $S = \mathcal{P}(n)$ using two-copy Clifford measurements, with sample complexity $O(n\log(n/\epsilon)/\epsilon^4)$ and runtime $\mathrm{poly}(2^n, 1/\epsilon)$.

**Theorem 10 ($k$-body fermionic, triply efficient).** Shadow tomography for $S = \mathcal{F}^{(n)}_k$ with $k = O(1)$, using two-copy Clifford measurements, sample complexity:
$$O\!\left(\frac{\log|\mathcal{F}^{(n)}_k|}{\epsilon^4} + \frac{p_k(4/\epsilon^2)\log|\mathcal{F}^{(n)}_k|}{\epsilon^2}\right) = O\!\left((k\log n)\,p_k(4/\epsilon^2)/\epsilon^2\right)$$

For $k=1$: $O((\log n)/\epsilon^4)$. For $k=2$: $O((\log n)/\epsilon^{18})$. For $k=3$: $O((\log n)/\epsilon^{110})$.

**Theorem 3 (Single-copy lower bound for fermions).** Any single-copy protocol for $\mathcal{F}^{(n)}_k$ requires $\Omega(n^k/\epsilon^2)$ copies — exponentially worse in $n$ than the two-copy protocol.

**Corollary 12 (Rapid-retrieval Pauli compression).** An $n$-qubit state $\rho$ can be compressed into $\mathrm{poly}(n)$ classical bits, from which $\mathrm{tr}(\rho P)$ for any Pauli $P$ can be extracted in $\mathrm{poly}(n)$ time, up to constant error $\epsilon = \Omega(1)$.

## Comparison with prior work

| Protocol | Observables | Copies/meas. | Sample complexity | Time efficient? |
|---|---|---|---|---|
| Random single-qubit Pauli [[Pauli Expectation Value Estimation\|classical shadows]] | $k$-local Paulis | 1 | $O(3^k k\log n / \epsilon^2)$ | ✓ |
| [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes\|Matchgate shadows]] | $k$-body fermionic | 1 | $O_k(n^k \log n / \epsilon^2)$ | ✓ |
| Bell sampling + gentle meas. (HKP 2021) | Any $S$ | Unrestricted | $O((\log\|S\|)/\epsilon^4)$ | ✓ |
| **This paper (Theorem 10)** | $k$-body fermionic | **2** | $(\log n) \cdot \mathrm{poly}_k(1/\epsilon)$ | ✓ |
| **This paper (Theorem 7)** | All Paulis | **2** | $O(n\log(n/\epsilon)/\epsilon^4)$ | ✓ |

The exponential separation between single-copy and two-copy protocols for fermionic operators is striking. [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Matchgate shadows]] achieve the optimal single-copy scaling $O(n^k/\epsilon^2)$ (up to logs), but that polynomial dependence on $n$ is inherent to single-copy approaches. Two-copy measurements eliminate it entirely.

## Limits / caveats

1. **$\epsilon$-dependence for $k \geq 2$ is brutal.** The polynomial $p_k(\omega)$ grows as $(2k\omega)^{(2k)^{k+1}}$, giving sample complexity $\sim \epsilon^{-O((2k)^{k+1})}$. For $k=2$: $\epsilon^{-18}$. For $k=3$: $\epsilon^{-110}$. Practical advantage over single-copy matchgate shadows requires very large $n$ relative to $1/\epsilon$.

2. **Rapid-retrieval compression requires constant $\epsilon$.** The Pauli compression (Corollary 12) only works for $\epsilon = \Omega(1)$. Whether $\epsilon = 1/\mathrm{poly}(n)$ is achievable with polynomial representation size remains open.

3. **Conjecture 13 (open).** The paper conjectures that the commutation graph of all Paulis with $|\mathrm{tr}(\rho P)| \geq \delta$ has fractional chromatic number $O(1/\delta^2)$. If true, this would give triply efficient shadow tomography for *any* subset of Paulis. The conjecture is equivalent to asking whether the fractional clique number (not just the clique number) is $O(1/\delta^2)$.

4. **Mimicking state computation is exponential.** The matrix multiplicative weights algorithm for Theorem 7 runs in $\mathrm{poly}(2^n)$ time, which is fine for learning all $4^n$ Paulis but would be a bottleneck for smaller sets requiring similar techniques.

5. **$k=1$ is the practical sweet spot.** The 1-body fermionic case has clean $O((\log n)/\epsilon^4)$ scaling and uses only edge coloring (Misra-Gries), making it genuinely implementable. Higher $k$ values require the full chi-boundedness induction with rapidly growing constants.

## Reusable ideas

1. [[Bell Difference Sampling for Magnitude Estimation]] — Bell measurements on $\rho \otimes \rho$ to learn $|\mathrm{tr}(\rho P)|$ for all Paulis simultaneously.
2. [[Fractional Coloring of Commutation Graphs]] — Reducing Pauli shadow tomography to fractional graph coloring of the commutation graph, with sample complexity linear in the fractional chromatic number.
3. [[Mimicking State via Matrix Multiplicative Weights]] — Computing a classical state $\sigma$ that matches the large-magnitude Pauli expectation values, then using Bell sampling on $\rho \otimes \sigma$ for sign recovery.
4. [[Clique Bound from Uncertainty Principle]] — Using the anticommutativity constraint $\sum_j \mathrm{tr}(P_j \rho)^2 \leq 1$ to bound the clique number of the post-Bell-sampling commutation graph.
5. [[Chi-Bounded Induction for Majorana Monomials]] — Recursive reduction of the fractional coloring problem for degree-$r$ Majorana monomials to degree $r-1$, yielding polynomial chi-binding functions independent of system size.

## References within this paper

- Aaronson, *Shadow tomography of quantum states* (STOC 2018) — introduced the shadow tomography problem
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins, Wan, McClean, Babbush et al. (2022)]] — the gradient-encoding approach to multi-observable estimation
- Huang, Kueng, Preskill, *Predicting many properties from few measurements* (2020) — classical shadows framework; the single-copy protocol for $k$-local Paulis
- Huang, Kueng, Preskill, *Information-theoretic bounds on quantum advantage in ML* (2021) — the Bell sampling + gentle measurement protocol for arbitrary Pauli sets
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Wan, Huggins, Lee, Babbush (2022)]] — matchgate shadows achieving optimal single-copy scaling for fermionic operators
- Bonet-Monroig, Babbush, O'Brien, *Nearly optimal measurement scheduling* (2020) — single-copy learning with ternary tree mapping; lower bounds for fermionic operators
- Chen, Cotler, Huang, Li (FOCS 2022) — proved single-copy impossibility for all Paulis ($\Omega(2^n/\epsilon^2)$ lower bound)
- Jiang, Kalev, Mruczkiewicz, Neven, *Optimal fermion-to-qubit mapping via ternary trees* (2020) — ternary tree mapping; single-copy learning matching fermionic lower bounds
- Gyárfás (1987) — seminal chi-boundedness result bounding chromatic number via longest induced path
- Arora, Kale (2007) — matrix multiplicative weights technique used for mimicking state
- Chen, Gong, Ye (2024) — independent concurrent work on Pauli tomography with limited memory

## Cross-links

### Paper notes
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — introduced the shadow tomography problem; this paper closes the gap to triple efficiency using two-copy measurements
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — optimal single-copy protocol for fermionic operators; this paper proves the $\Omega(n^k/\epsilon^2)$ lower bound that matchgate shadows saturate, and then beats it exponentially with two copies
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — gradient-based multi-observable estimation; different regime (high precision, oracle access) but same problem domain
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — measurement grouping for VQE; the fractional coloring perspective here generalizes the Pauli grouping strategies used there
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — first-quantized classical shadows for $k$-RDM; complementary approach to RDM learning
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — QC-AFQMC uses shadow tomography for overlap estimation; the improved fermionic tomography here could reduce sample costs
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]] — Kothari and Babbush; different problem (planted inference vs. tomography) but same Google Quantum AI group, same era
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — uses shadow tomography (classical shadows) for projected quantum kernels; the two-copy protocols here could improve those constructions

### Trick cards
- [[Matchgate 3-Design for Classical Shadows]] — the single-copy fermionic shadow protocol that this paper's two-copy protocol supersedes in sample complexity
- [[First-Quantized Classical Shadows for k-RDM]] — alternative RDM measurement approach in the first-quantized setting
- [[Shadow Tomography for QMC Overlap Estimation]] — application of shadow tomography in QC-AFQMC
- [[Pfaffian Shadow Estimation for Fermionic Observables]] — efficient post-processing for matchgate shadows
- [[Basis Rotation Grouping for VQE Measurement]] — deterministic Pauli grouping; can be viewed as a special case of the coloring framework
- [[Bravyi-Kitaev Transformation]] — one of the fermion-to-qubit mappings discussed
