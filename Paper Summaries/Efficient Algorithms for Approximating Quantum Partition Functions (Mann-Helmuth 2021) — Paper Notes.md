> **Source:** Ryan L. Mann and Tyler Helmuth, *Efficient Algorithms for Approximating Quantum Partition Functions*, Journal of Mathematical Physics **62**(2), 022201 (2021)
> **Links:** [arXiv:2004.11568](https://arxiv.org/abs/2004.11568) · [Journal](https://doi.org/10.1063/5.0029986)
> **Tags:** #partition-function #cluster-expansion #classical-algorithm #approximate-counting #complexity

---

## The computational problem

Given a quantum spin system on a bounded-degree graph $G = (V, E)$ with maximum degree $\Delta$, each vertex hosting a $d$-dimensional Hilbert space, and a pairwise interaction $\Phi$ with $\|\Phi(e)\| \leq 1$, compute the quantum partition function

$$Z_G(\beta) = \mathrm{Tr}\left(e^{-\beta H_G}\right), \qquad H_G = \sum_{e \in E} \Phi(e)$$

to multiplicative accuracy $\varepsilon$ in time polynomial in $|V|$ and $1/\varepsilon$.

This is a **classical** computation problem — no quantum computer involved. The question is whether the partition function, which encodes all thermodynamic information about the quantum system, can be efficiently approximated classically.

---

## What the paper does

Establishes a fully polynomial-time approximation scheme (FPTAS) for $Z_G(\beta)$ on bounded-degree graphs whenever the inverse temperature satisfies $|\beta| \leq 1/(e^4 \Delta)$. The algorithm is based on the quantum cluster expansion of Netočný and Redig, combined with the algorithmic framework of Helmuth, Perkins, and Regts for converting convergent cluster expansions into efficient algorithms.

The main contribution is a simple, self-contained proof that improves the convergence radius over prior work — from $|\beta| \leq (10e^2 \Delta)^{-1}$ (Harrow-Mehraban-Soleimanifar, quasi-poly time) and $|\beta| \leq (16e^3 \Delta)^{-1}$ (Kuwahara-Kato-Brandão, poly time) to $|\beta| \leq (e^4 \Delta)^{-1}$ with polynomial runtime. The paper is only 7 pages — it's a masterclass in clean exposition.

---

## The algorithm / construction

### Step 1: Polymer model representation

A **polymer** $\gamma$ is a multiset of edges whose support induces a connected subgraph of $G$. Two polymers are **compatible** if their supporting subgraphs are vertex-disjoint. The partition function admits an abstract polymer representation:

$$Z_G(\beta) = \sum_{\Gamma \in \mathcal{G}} \prod_{\gamma \in \Gamma} w_\gamma$$

where $\mathcal{G}$ is the set of all admissible (pairwise compatible) polymer sets, and the polymer weight is:

$$w_\gamma = \frac{(-\beta)^{\|\gamma\|}}{\|\gamma\|! \prod_{e \in E_\gamma} m_\gamma(e)!} \mathrm{Tr}\left(\sum_{\sigma \in S_{\|\gamma\|}} \prod_{i=1}^{\|\gamma\|} \Phi(\gamma_{\sigma(i)})\right)$$

Here $\|\gamma\| = \sum_e m_\gamma(e)$ is the polymer size. This follows from expanding $e^{-\beta H}$ as a Taylor series and grouping terms by connected components.

### Step 2: Cluster expansion

Taking the logarithm gives the cluster expansion:

$$\log Z_G(\beta) = \sum_{\Gamma \in \mathcal{G}^C} \varphi(H_\Gamma) \prod_{\gamma \in \Gamma} w_\gamma$$

where $\mathcal{G}^C$ is the set of all **clusters** (ordered tuples of polymers whose incompatibility graph is connected) and $\varphi(H)$ is the **Ursell function** (related to the Möbius function of the partition lattice).

### Step 3: Truncation

The truncated cluster expansion to order $m$:

$$T_m(Z_G(\beta)) = \sum_{\substack{\Gamma \in \mathcal{G}^C \\ \|\Gamma\| < m}} \varphi(H_\Gamma) \prod_{\gamma \in \Gamma} w_\gamma$$

When $|\beta| \leq 1/(e^4 \Delta)$, the truncation error satisfies $|T_m - \log Z_G(\beta)| \leq |V| e^{-m}$, so taking $m = \log(|V|/\varepsilon)$ gives a multiplicative $\varepsilon$-approximation.

### Step 4: Efficient evaluation

Three ingredients make $T_m$ computable in $\exp(O(m)) \cdot |V|^{O(1)}$ time:
1. **Enumerate clusters** of size $\leq m$ in $\exp(O(m)) \cdot |V|^{O(1)}$ time (via connected subgraph enumeration)
2. **Compute Ursell functions** in $\exp(O(|V(H)|))$ time (Björklund et al.)
3. **Compute polymer weights** in $\exp(O(\|\gamma\|))$ time (via Ryser's formula for the permanent to handle the symmetrisation, then diagonalisation)

Since $m = O(\log(|V|/\varepsilon))$, the total runtime is polynomial in $|V|$ and $1/\varepsilon$.

---

## Key results

**Theorem 1.** For any fixed $\Delta \in \mathbb{Z}^+$, there is an FPTAS for $Z_G(\beta)$ on all graphs $G$ of maximum degree $\leq \Delta$ and all complex $\beta$ with $|\beta| \leq 1/(e^4 \Delta)$.

**Runtime:** $\exp(O(\log(|V|/\varepsilon))) \cdot |V|^{O(1)} = |V|^{O(\log(d\Delta))} \cdot (1/\varepsilon)^{O(\log(d\Delta))}$ — polynomial, but with a degree that grows with $\log(d\Delta)$.

**Convergence bound (Lemma 4):** For $|\beta| \leq 1/(e^4 \Delta)$, the cluster expansion converges absolutely and $|T_m - \log Z_G| \leq |V| e^{-m}$.

---

## Comparison with prior work

| Paper | Convergence radius | Runtime | Method |
|-------|--------------------|---------|--------|
| Harrow-Mehraban-Soleimanifar (2020) | $\|\beta\| \leq (10e^2 \Delta)^{-1}$ | Quasi-polynomial | Cluster expansion + Barvinok's method |
| Kuwahara-Kato-Brandão (2020) | $\|\beta\| \leq (16e^3 \Delta)^{-1}$ | Polynomial | Quantum belief propagation |
| **This paper** | $\|\beta\| \leq (e^4 \Delta)^{-1}$ | Polynomial | Cluster expansion (Kotecký-Preiss) |

The improvement in the convergence radius constant ($e^4 \approx 54.6$ vs $16e^3 \approx 321$ and $10e^2 \approx 73.9$) is modest but the proof is significantly simpler. The $O(1/\Delta)$ scaling is optimal under $\mathrm{RP} \neq \mathrm{NP}$ — there are hardness results showing that approximate counting becomes intractable for $\beta = \Omega(1/\Delta)$.

---

## Limits / caveats

1. **High temperature only.** The algorithm works when $|\beta| = O(1/\Delta)$, which is the disordered / high-temperature regime. Low-temperature behaviour — where the interesting physics lives — is not covered. (The sequel [[Efficient Algorithms for Approximating Quantum Partition Functions at Low Temperature (Helmuth-Mann 2023) — Paper Notes|Helmuth-Mann (2023)]] addresses this for stable quantum perturbations of classical systems.)

2. **Polynomial degree.** The runtime polynomial has degree $O(\log(d\Delta))$, which can be large for high-dimensional local Hilbert spaces. The paper notes that Markov chain polymer methods might improve this.

3. **Pairwise interactions only.** The main theorem is stated for graphs (pairwise), though the cluster expansion formalism works for bounded-rank hypergraphs with some modifications.

4. **Classical algorithm.** This doesn't say anything about quantum speedups for partition functions — it shows that in the high-temperature regime, the problem is classically tractable, so there's no quantum advantage to be found here.

5. **Complex $\beta$ handled.** The convergence holds for complex inverse temperature, which connects to computational hardness transitions via Lee-Yang zeros.

---

## Reusable ideas

1. **[[Quantum Cluster Expansion for Partition Functions]]** — Representing $Z_G(\beta)$ as a sum over compatible polymer sets where each polymer is a connected multiset of edges, then taking the logarithm to get a cluster expansion that converges when the coupling is weak. This is the quantum generalisation of the classical Mayer expansion.

2. **[[Truncated Cluster Expansion as FPTAS]]** — Converting a convergent cluster expansion into an efficient algorithm by truncating at order $m = O(\log(n/\varepsilon))$ and using the fact that clusters of bounded size can be enumerated in singly-exponential time. The paradigm is: convergence + efficient enumeration + efficient weight computation = FPTAS.

3. **[[Ryser Formula for Symmetrised Operator Products]]** — Computing $\mathrm{Tr}\left(\sum_{\sigma \in S_n} \prod_i \Phi(\gamma_{\sigma(i)})\right)$ via the inclusion-exclusion identity $\sum_{\sigma} \prod_i A_{\sigma(i)} = (-1)^n \sum_{A \subseteq [n]} (-1)^{|A|} \left(\sum_{i \in A} A_i\right)^n$, reducing from $n!$ terms to $2^n$. Analogous to Ryser's formula for the permanent.

---

## References within this paper

- **Netočný and Redig (2004)** — the quantum cluster expansion that provides the polymer model representation; the foundation of the whole approach
- **Kotecký and Preiss (1986)** — abstract polymer model and cluster expansion formalism
- **Helmuth, Perkins, and Regts (2020)** — the algorithmic framework for converting convergent cluster expansions into FPTAS; classical predecessor
- **Harrow, Mehraban, and Soleimanifar (2020)** — prior quantum partition function FPTAS with worse convergence radius and quasi-poly runtime; arXiv:1910.09071
- **Kuwahara, Kato, and Brandão (2020)** — prior poly-time algorithm via quantum belief propagation; PRL 124, 220601; arXiv:1910.09425
- [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes|Mann and Bremner (2019)]] — same authors; complexity transitions for complex Ising partition functions
- **Barvinok (2016)** — interpolation method for partition function approximation; alternative to cluster expansion approach
- **Björklund, Husfeldt, Kaski, Koivisto (2008)** — efficient computation of the Ursell function

---

## Cross-links

### Paper notes
- [[Efficient Algorithms for Approximating Quantum Partition Functions at Low Temperature (Helmuth-Mann 2023) — Paper Notes|Helmuth-Mann (2023)]] — sequel: extends to low-temperature stable perturbations of classical systems
- [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes|Mann-Bremner (2019)]] — same first author; complexity landscape for Ising models
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — quantum speedup for partition function estimation via quantum mean estimation (orthogonal approach: quantum algorithm vs classical FPTAS)

### Trick cards
- [[Quantum Cluster Expansion for Partition Functions]]
- [[Truncated Cluster Expansion as FPTAS]]
- [[Ryser Formula for Symmetrised Operator Products]]
- [[Telescoping Product Estimation for Partition Functions]] — different approach to partition functions (quantum-algorithmic)
