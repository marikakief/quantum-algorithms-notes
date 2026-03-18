> **Source:** Andris Ambainis, Andrew M. Childs, Ben W. Reichardt, Robert Špalek, Shengyu Zhang, *Every AND-OR formula of size N can be evaluated in time $N^{1/2+o(1)}$ on a quantum computer*, FOCS 2007, pp. 363–372. Based on two companion arXiv papers:
> - Childs, Reichardt, Špalek, Zhang: arXiv:quant-ph/0703015
> - Ambainis: arXiv:0704.3628
> **Links:** [arXiv (CRŠ Z)](https://arxiv.org/abs/quant-ph/0703015) · [arXiv (Ambainis)](https://arxiv.org/abs/0704.3628) · [FOCS](https://doi.org/10.1109/FOCS.2007.57)
> **Tags:** #quantum-walk #formula-evaluation #query-complexity #game-trees #foundational

---

## The problem

Evaluate a Boolean formula $\varphi$ of size $N$ (counting input variables with multiplicity) built from AND, OR, NOT gates — equivalently, NAND gates — using black-box access to the input bits. What's the quantum query complexity?

This is the decision version of evaluating a game tree: AND-OR formulas capture two-player game evaluation (MIN-MAX trees). Classically, balanced binary AND-OR trees require $\Theta(N^{0.754\ldots})$ queries (alpha-beta pruning). The quantum lower bound is $\Omega(\sqrt{N})$ (adversary method).

## What the paper does

**Theorem:** Any NAND formula of size $N$ can be evaluated with bounded error using $N^{1/2 + o(1)}$ quantum queries. For balanced or "approximately balanced" formulas, the query complexity is $O(\sqrt{N})$, which is optimal.

The algorithm is based on a **coined quantum walk** on the formula tree, with phase estimation to distinguish the two cases ($\varphi(x) = 0$ vs. $\varphi(x) = 1$). The analysis connects the coined walk to a continuous-time quantum walk via [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy's quantization theorem]].

This almost resolves the Laplante-Lee-Szegedy conjecture: the formula size of a Boolean function $f$ is at least $Q_2(f)^{2 - o(1)}$, where $Q_2(f)$ is the bounded-error quantum query complexity.

---

## The algorithm

### Setup

Given a NAND formula $\varphi$ of size $N$, represent it as a rooted tree $T$ where leaves are inputs and internal nodes are NAND gates. Attach a two-vertex "tail" ($r', r''$) below the root $r$.

### Weighted adjacency Hamiltonian

Define a symmetric weighted adjacency matrix $H$ on $T$ (plus tail):

$$H_{pv} = \left(\frac{s_v}{s_p}\right)^{1/4}$$

where $s_v$ is the number of inputs under vertex $v$. Two exceptions:
1. If leaf $v$ evaluates to 1 ($x_v = 1$), set $H_{pv} = 0$ — disconnect it
2. The tail edge $H_{r'' r'}$ gets a special weight involving $\sigma_-(r)$ and $N^{1/4}$

The input dependence enters only through which leaves are disconnected.

### Key spectral properties (Theorem 2)

- **If $\varphi(x) = 0$:** There exists a zero-energy eigenstate $|a\rangle$ of $H$ with overlap $\langle r'' | a \rangle \geq 1/\sqrt{2}$
- **If $\varphi(x) = 1$:** Every eigenstate with support on $r'$ or $r''$ has $|E| \geq \frac{1}{9\sigma_-(r)\sqrt{\sigma_+(r)}}$

So the formula's value is encoded in whether or not $H$ has a zero-energy eigenstate overlapping the tail.

### From Hamiltonian to coined walk

Use [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy's theorem]] (Theorem 7) to convert $H$ into a coined quantum walk $U = (2\Pi - I)S$:
- Factor $H = P \circ P^T$ where $P$ is a row-stochastic matrix (Claim 8)
- The walk operator $U$ acts on directed edges: swap $S$, then reflect about the coin state $|\tilde{v}\rangle$ at each vertex
- Eigenvalues of $U$ are $\beta_{a,\pm} = -\lambda_a \pm i\sqrt{1 - \lambda_a^2}$ where $\lambda_a$ are eigenvalues of $H$

The spectral gap of $H$ near zero maps to a phase gap of $U$ near $\pm i$.

### Phase estimation

Run [[Gapped Phase Estimation|phase estimation]] on $U$ starting from $|\tilde{r}''\rangle = |r'', r'\rangle$:
- **$\varphi(x) = 0$:** There's an eigenvalue at phase $\pi/2$ (or $3\pi/2$) with large overlap → phase estimation outputs 0 or $T/2$ with probability $\geq 1/4$
- **$\varphi(x) = 1$:** All relevant eigenphases are bounded away from $\pi/2$ → phase estimation outputs 0 or $T/2$ with probability $< 1/4$

### Handling unbalanced formulas

For unbalanced formulas, the spectral gap can be small. Apply the Bshouty-Cleve-Eberly / Bonet-Buss **rebalancing** (Lemma 10): for parameter $k$, produce an equivalent formula with depth $O(k \log N)$ and size $N^{1 + 1/\log_2 k}$. Optimise $k = 2^{\sqrt{\log_2 N / 3}}$ to get $N^{1/2 + O(1/\sqrt{\log N})}$ total queries.

---

## The balanced case in detail

For a balanced binary NAND tree ($N = 2^n$, depth $n$), the full algorithm (Figure 2 of the paper) is clean:

1. Prepare $\frac{1}{\sqrt{T}} \sum_{t=0}^{T-1} (-i)^t |t\rangle \otimes |r''\rangle |{\text{left}}\rangle$ with $T = 320 \lfloor \sqrt{N} \rfloor$
2. Apply $t$ steps of the coined walk $U$ controlled on the counter:
   - **Diffusion step:** At each vertex, apply reflection about the uniform coin state $|u\rangle = \frac{1}{\sqrt{3}}(|{\text{down}}\rangle + |{\text{left}}\rangle + |{\text{right}}\rangle)$; at leaves, first apply a phase flip $(−1)^{x_v}$ via oracle query; at $r'$, use a biased coin
   - **Walk step:** Move along edges according to the coin direction
3. Inverse QFT on the counter, measure: output 0 iff result is 0 or $T/2$

Query complexity: $O(\sqrt{N})$. Optimal.

---

## Key results

| Variant | Query complexity | Notes |
|---|---|---|
| Balanced binary NAND/AND-OR | $O(\sqrt{N})$ | Optimal (matches $\Omega(\sqrt{N})$ lower bound) |
| Approximately balanced | $O(\sqrt{N})$ | $\sigma_-(r) = O(1)$, $\sigma_+(r) = O(N)$ |
| Arbitrary NAND formula, depth $d$ | $O(\sqrt{Nd})$ | Via continuous-time walk |
| General size-$N$ formula | $N^{1/2 + O(1/\sqrt{\log N})}$ | After rebalancing |

**Corollary:** Formula size of $f$ is $\geq Q_2(f)^{2 - o(1)}$, almost resolving the Laplante-Lee-Szegedy conjecture.

---

## Historical context

| Work | Contribution |
|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes\|Grover (1996)]] | $O(\sqrt{N})$ for OR of $N$ bits |
| Buhrman-Cleve-Wigderson (1998) | Nested Grover for depth-$d$ AND-OR in $\sqrt{N} \cdot O(\log N)^{d-1}$ |
| Høyer-Mosca-de Wolf (2003) | Constant-depth AND-OR in $O(\sqrt{N})$ via noisy Grover |
| Barnum-Saks (2004) | $\Omega(\sqrt{N})$ lower bound for any AND-OR formula |
| Farhi-Goldstone-Gutmann (2007) | $O(\sqrt{N})$ for balanced NAND tree in continuous-time model |
| Childs-Cleve-Jordan-Yeung (2007) | Discretisation of FGG to $N^{1/2+\varepsilon}$ |
| **This paper (2007)** | **$O(\sqrt{N})$ for balanced, $N^{1/2+o(1)}$ for general, discrete-query model** |

---

## Limits / caveats

- The $o(1)$ in the exponent for general formulas comes from formula rebalancing. Whether $O(\sqrt{N})$ suffices for all formulas (not just balanced ones) was left open. (Reichardt later achieved this for read-once formulas via span programs.)
- Preprocessing requires knowing the formula structure (to compute coin biases from the principal eigenvector of $H$). The classical preprocessing takes $\mathrm{poly}(N)$ time.
- The algorithm doesn't find a *winning strategy* in a game tree — it only evaluates whether one exists.

---

## Reusable ideas

1. [[Szegedy Walk for Formula Evaluation]] — use Szegedy's quantization to convert a weighted adjacency Hamiltonian $H$ into a coined quantum walk $U$, then use phase estimation to detect zero-energy eigenstates. The formula's value is encoded in the spectral gap near zero.
2. [[Formula Rebalancing for Quantum Evaluation]] — classically preprocess an unbalanced formula into a balanced one (depth $O(k \log N)$, size $N^{1+1/\log k}$) to ensure the spectral gap is polynomially large, at the cost of a sub-polynomial blowup in formula size.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — OR evaluation as the base case
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — the quantization theorem (Theorem 7) connecting classical walks to quantum walks
- Farhi, Goldstone, Gutmann (2007, quant-ph/0702144) — continuous-time walk algorithm for balanced NAND trees; the direct inspiration
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs et al. (2003)]] — first exponential quantum walk speedup; same research lineage
- Barnum & Saks (2004) — $\Omega(\sqrt{N})$ lower bound via adversary method
- Laplante, Lee, Szegedy (2006) — the formula-size vs. query-complexity conjecture this paper almost resolves
- Saks & Wigderson (1986) — classical $\Theta(N^{0.754})$ for balanced AND-OR trees

---

## Cross-links

### Paper notes
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — Szegedy's quantization theorem is the backbone of the analysis
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — first exponential walk speedup; same authors, same lineage
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]] — generalises the continuous-to-discrete walk correspondence used here
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — OR evaluation as the $k=1$ case
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — Ambainis's other major quantum walk result
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]] — survey of quantum walk algorithms

### Trick cards
- [[Szegedy Walk for Formula Evaluation]]
- [[Formula Rebalancing for Quantum Evaluation]]
- [[Coined Quantum Walk Search on Graphs]]
- [[Gapped Phase Estimation]]
- [[Column-Subspace Reduction for Symmetric Graphs]] — related symmetry-based reduction for walk analysis
