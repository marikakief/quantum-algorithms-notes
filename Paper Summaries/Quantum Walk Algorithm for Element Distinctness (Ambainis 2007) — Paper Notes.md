> **Source:** Andris Ambainis, *Quantum walk algorithm for element distinctness*, SIAM J. Comput. **37**(1), 210–239 (2007); arXiv:quant-ph/0311001
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0311001) · [SIAM](https://doi.org/10.1137/S0097539705447311)
> **Tags:** #element-distinctness #quantum-walk #Johnson-graph #query-complexity #optimal

---

## The computational problem

**Element distinctness:** Given $x_1, \ldots, x_N \in [M]$, are there distinct $i, j$ with $x_i = x_j$?

**Element $k$-distinctness:** Are there $k$ distinct indices $i_1, \ldots, i_k$ with $x_{i_1} = \cdots = x_{i_k}$?

| Algorithm | Queries | Notes |
|---|---|---|
| Classical (sorting) | $\Theta(N)$ | Optimal classically |
| [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes|Buhrman et al. (2001)]] | $O(N^{3/4})$ | First quantum improvement |
| [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes\|BHT]] (collision, $r$-to-1) | $O(N^{1/3})$ | Requires $r$-to-1 promise |
| Aaronson-Shi lower bound | $\Omega(N^{2/3})$ | Polynomial method |
| **This paper** | $O(N^{2/3})$ | **Tight** |

---

## What the paper does

Gives an $O(N^{2/3})$ quantum query algorithm for element distinctness, matching the Aaronson-Shi lower bound. The technique — quantum walk on the Johnson graph — became the template for a whole family of quantum algorithms. Also gives $O(N^{k/(k+1)})$ for $k$-distinctness.

---

## The algorithm

### The graph

Vertices: subsets $S \subseteq [N]$ of size $r$ (and $r+1$, for the walk's intermediate states). A vertex $v_S$ is **marked** if $S$ contains a collision ($i, j \in S$ with $x_i = x_j$).

Edges: $v_S$ and $v_T$ are adjacent if $|S \triangle T| = 2$ (replace one element).

This is a subgraph of the Johnson graph $J(N, r)$.

### Why not just Grover?

Naive approach: Grover search over all $\binom{N}{r}$ subsets of size $r$. Each vertex requires $r$ queries to check. With $r = N^{2/3}$, fraction of marked vertices $\approx r^2/N^2 = N^{-2/3}$, so Grover needs $O(N^{1/3})$ iterations, each costing $r = N^{2/3}$ queries → total $O(N)$. No speedup.

**The fix:** Reuse data between adjacent vertices. Moving from $v_S$ to $v_T$ (differing in one element) costs only 1 query. The quantum walk amortises the setup cost.

### The walk (Algorithm 1)

State space: $|S, x, y\rangle$ where $S$ is an $r$-element subset, $x = (x_i)_{i \in S}$ is the stored data, and $y \notin S$ indexes the "next element to swap in."

One step of the walk:

1. Apply Grover diffusion on the $y$ register (over elements not in $S$) — this is the "coin flip"
2. Map from $\mathcal{H}$ to $\mathcal{H}'$: add $y$ to $S$, query $x_y$
3. Apply Grover diffusion on the $y$ register (now over elements in $S$)
4. Remove new $y$ from $S$, erase $x_y$ via uncomputation
5. Map back to $\mathcal{H}$

**Queries per step:** 2 (one to load $x_y$ in step 2, one to erase in step 4). Effectively 1 "net" query.

### The search (Algorithm 2 — single collision)

1. Prepare uniform superposition over all $(S, y)$ with $|S| = r$
2. Query all $x_i$ for $i \in S$ — cost: $r$ queries
3. Repeat $t_1 = O((N/r)^{k/2})$ times:
   - (a) Phase flip: $|S, y, x\rangle \to -|S, y, x\rangle$ if $S$ contains a $k$-collision
   - (b) Perform $t_2 = O(\sqrt{r})$ walk steps
4. Measure. Check if $S$ contains a collision.

**Total queries:** $r + t_1 \cdot t_2 \cdot O(1) = r + O\left(\frac{N^{k/2}}{r^{(k-1)/2}}\right)$

Optimise: $r = N^{k/(k+1)}$, total = $O(N^{k/(k+1)})$.

For element distinctness ($k = 2$): $r = N^{2/3}$, total = $O(N^{2/3})$.

---

## The analysis: why it works

### The $(2k+1)$-dimensional invariant subspace

The key structural insight: the algorithm's state always lives in a $(2k+1)$-dimensional subspace $\tilde{\mathcal{H}}$.

Classify basis states $|S, y\rangle$ by the pair $(j, l)$ where:
- $j = |S \cap \{i_1, \ldots, i_k\}|$ (how many collision elements are in $S$)
- $l = 1$ if $y \in \{i_1, \ldots, i_k\}$, $l = 0$ otherwise

The uniform superposition of states of each type $(j, l)$ gives the basis $|\psi_{j,l}\rangle$, $j = 0, \ldots, k$, $l \in \{0, 1\}$ (with $(k, 1)$ absent). These span $\tilde{\mathcal{H}}$.

**Lemma 1:** The quantum walk maps $\tilde{\mathcal{H}}$ to itself. This is because the Grover diffusion and the walk steps preserve the symmetry of the type classification.

### Eigenvalues of the walk (Lemma 2)

The walk unitary restricted to $\tilde{\mathcal{H}}$ has eigenvalues $1$ (eigenvector = $|\psi_{\text{start}}\rangle$) and $e^{\pm i\theta_j}$ with:

$$
\theta_j = \frac{2\sqrt{j} + o(1)}{\sqrt{r}}, \qquad j = 1, \ldots, k
$$

The eigenvalue decomposition reduces to $2 \times 2$ blocks via the matrix $D_\epsilon = \begin{pmatrix} 1 - 2\epsilon & 2\sqrt{\epsilon - \epsilon^2} \\ 2\sqrt{\epsilon - \epsilon^2} & -1 + 2\epsilon \end{pmatrix}$ for $\epsilon = j/(N-r)$ and $j/(r+1)$.

Setting $t_2 = \lceil \frac{\pi}{3\sqrt{k}} \sqrt{r} \rceil$ makes all non-trivial eigenvalues $e^{i\theta}$ with $\theta \in [c, 2\pi - c]$ for a constant $c$ — bounded away from 1.

### The generalised Grover lemma (Lemma 3)

The main technical contribution. Given:
- $U_1$: phase flip on marked states (like Grover's oracle)
- $U_2$: unitary with eigenvalue 1 for $|\psi_{\text{start}}\rangle$ and all other eigenvalues $e^{i\theta}$ with $\theta \in [\epsilon, 2\pi - \epsilon]$, $\theta \neq \pi$
- $|\langle\psi_{\text{good}}|\psi_{\text{start}}\rangle| = \alpha$

Then $\exists\, t = O(1/\alpha)$ such that $|\langle\psi_{\text{good}}|(U_2 U_1)^t |\psi_{\text{start}}\rangle| = \Omega(1)$.

This generalises Grover's analysis: $U_2$ doesn't need to be the exact reflection about $|\psi_{\text{start}}\rangle$ — any unitary that fixes $|\psi_{\text{start}}\rangle$ and has a spectral gap works. Childs and Eisenberg (2005) later improved this to show success probability $1 - o(1)$.

### Putting it together

The marked fraction $\alpha' = \Pr[\{i_1, \ldots, i_k\} \subseteq S] = \Theta(r^k/N^k)$, so $\alpha = \sqrt{\alpha'} = \Theta(r^{k/2}/N^{k/2})$.

By Lemma 3: $t_1 = O(1/\alpha) = O((N/r)^{k/2})$ phase flip + walk cycles suffice.

Total queries: $r + t_1 \cdot t_2 = r + O\left(\frac{N^{k/2}}{r^{(k-1)/2}}\right)$.

---

## Space-time tradeoff

For memory budget $r < N^{2/3}$:

$$
\text{Queries} = O\left(\max\left(r, \frac{N}{\sqrt{r}}\right)\right)
$$

| Memory $r$ | Queries |
|---|---|
| $N^{2/3}$ | $O(N^{2/3})$ (optimal) |
| $\sqrt{N}$ | $O(N^{3/4})$ (matches Buhrman et al.) |
| $1$ | $O(N)$ (no advantage) |

---

## Impact and extensions

This paper launched the **quantum walk search** paradigm:
- **Magniez, Santha, Szegedy (2005):** Triangle finding in $O(n^{1.3})$ using element distinctness as subroutine
- **Ambainis, Kempe, Rivosh (2005):** Coined walk search on grids; coins make walks faster
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|**Szegedy (2004):**]] Generalised the walk framework to arbitrary Markov chains; quadratic speedup for spectral gap
- **Buhrman, Špalek (2006):** Matrix verification in $O(n^{5/3})$ via Szegedy's framework
- **Childs, Eisenberg (2005):** Alternative analysis showing success probability $1 - o(1)$

---

## Reusable ideas

1. **[[Walk on the Johnson Graph for Subset Search]]:** Walk on subsets of $[N]$, storing queried values. Moving between adjacent subsets costs 1 query; checking if marked costs 0. Balances setup cost $r$ against walk cost $N^{k/2}/r^{(k-1)/2}$ for $k$-element problems.

2. **[[Generalised Grover Lemma for Quantum Walks]]:** In a Grover-like sequence $(U_2 U_1)^t$, $U_2$ need not be the exact reflection about the start state — any unitary fixing $|\psi_{\text{start}}\rangle$ with a spectral gap works. The iteration count is $O(1/\alpha)$ where $\alpha = |\langle\psi_{\text{good}}|\psi_{\text{start}}\rangle|$.

---

## References within this paper

- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard, Høyer & Tapp (1997)]] — collision finding ($O(N^{1/3})$) with $r$-to-1 promise
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Boyer, Brassard, Høyer & Tapp (1998)]] — tight bounds on quantum search
- Aaronson & Shi (2004) — $\Omega(N^{2/3})$ lower bound via polynomial method
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs & Goldstone (2004)]] — continuous walk search on lattices
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — generalised the walk framework to Markov chains
- [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes|Buhrman, Dürr, Heiligman, Høyer, Magniez, Santha & de Wolf (2001)]] — previous $O(N^{3/4})$ algorithm via nested amplitude amplification
- Childs & Eisenberg (2005) — improved analysis of Algorithm 2
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — gives continuous-time element distinctness via walk–Hamiltonian correspondence

---

## Cross-links

### Paper notes
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]] — Ambainis's adversary method; proves $\Omega(N^{2/3})$ for element distinctness as an application
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]] — polynomial method; the Aaronson-Shi $\Omega(N^{2/3})$ lower bound uses polynomial degree arguments
- [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]] — predecessor: $O(N^{3/4})$ via nested amplitude amplification
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]] — Ambainis's own survey covering this work
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — abstracts the walk framework to arbitrary Markov chains
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]] — continuous walk approach
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]] — collision finding with structure
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] — generalises this approach to arbitrary ergodic chains; handles multiple solutions without reduction

### Trick cards
- [[Walk on the Johnson Graph for Subset Search]]
- [[Generalised Grover Lemma for Quantum Walks]]
- [[Coined Quantum Walk Search on Graphs]]
- [[Standard Amplitude Amplification]]
- [[Classical Preprocessing plus Grover Search]]
