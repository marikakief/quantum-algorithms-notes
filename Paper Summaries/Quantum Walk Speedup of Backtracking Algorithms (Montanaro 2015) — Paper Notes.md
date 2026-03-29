> **Source:** Ashley Montanaro, *Quantum walk speedup of backtracking algorithms*, arXiv:1509.02374, Theory of Computing **14**(15):1–24, 2018
> **Links:** [arXiv](https://arxiv.org/abs/1509.02374) · [Journal](https://doi.org/10.4086/toc.2018.v014a015)
> **Tags:** #quantum-walk #backtracking #constraint-satisfaction #CSP #DPLL #search #Belovs

---

## The computational problem

**Input:** A constraint satisfaction problem (CSP) on $n$ variables over domain $[d]$, defined by:
- A predicate $P : ([d] \cup \{*\})^n \to \{\text{true}, \text{false}, \text{indeterminate}\}$ that can evaluate partial assignments
- A heuristic $h$ that determines which variable to branch on next

**Output:** A complete assignment $x$ such that $P(x) = \text{true}$, or certification that none exists.

**Classical algorithm:** A generic backtracking algorithm (Algorithm 1 in the paper) explores a tree whose vertices are partial solutions. Internal vertices are "indeterminate" assignments; leaves are either solutions ($P = \text{true}$) or dead ends ($P = \text{false}$). Let $T$ be the number of vertices in this tree. Each vertex costs one evaluation of $P$ and $h$, so the classical runtime is $O(T \cdot \text{poly}(n))$.

This framework covers DPLL (the backbone of modern SAT solvers), DSATUR for graph colouring, and essentially any CSP algorithm that proceeds by branching-and-pruning.

---

## What the paper does

Shows that *any* classical backtracking algorithm in the above framework can be sped up quantumly: if the classical backtracking tree has $T$ vertices, the quantum algorithm uses $O(\sqrt{T} \cdot n^{3/2} \log n)$ evaluations of $P$ and $h$ to find a solution (or $O(\sqrt{T} n)$ to detect one). The speedup is near-quadratic — the $n^{3/2}$ overhead comes from binary search to locate the solution within the tree, and is typically dwarfed by $\sqrt{T}$ since $T$ is exponential in $n$.

The key insight: rather than applying [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover's algorithm]] to search over all $d^n$ complete assignments, search within the (potentially much smaller) backtracking tree using a quantum walk. This respects the structure that makes backtracking efficient classically while still providing a quadratic quantum speedup.

---

## The algorithm

### Setup: quantum walk on the backtracking tree

The quantum walk operates on the Hilbert space $\mathcal{H}$ spanned by $\{|r\rangle\} \cup \{|x\rangle : x \in \{1, \ldots, T-1\}\}$, starting at the root $|r\rangle$. Unlike the [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy walk]], no coin register is needed — the walk is "coinless."

Partition the vertices into sets $A$ (even distance from root) and $B$ (odd distance). Define local diffusion operators $D_x$ on the subspace of $x$ and its children:

- **Marked vertex:** $D_x = I$ (identity — do nothing)
- **Unmarked, $x \neq r$:** $D_x = I - 2|\psi_x\rangle\langle\psi_x|$, where $|\psi_x\rangle = \frac{1}{\sqrt{d_x}}\left(|x\rangle + \sum_{y: x \to y} |y\rangle\right)$
- **Root:** $D_r = I - 2|\psi_r\rangle\langle\psi_r|$, where $|\psi_r\rangle = \frac{1}{\sqrt{1 + d_r n}}\left(|r\rangle + \sqrt{n}\sum_{y: r \to y} |y\rangle\right)$

The root diffusion has an extra $\sqrt{n}$ weighting on its children. This is needed so the eigenvector with eigenvalue 1 (when a marked vertex exists) has large overlap with $|r\rangle$.

One step of the walk is $R_B R_A$, where $R_A = \bigoplus_{x \in A} D_x$ and $R_B = |r\rangle\langle r| + \bigoplus_{x \in B} D_x$.

### Detection algorithm

Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] to the walk operator $R_B R_A$ with precision $\beta / \sqrt{Tn}$:

1. Repeat $K = O(\log(1/\delta))$ times:
   - Run phase estimation on $R_B R_A$ starting from $|r\rangle$
   - Accept if the eigenvalue is 1, reject otherwise
2. If the acceptance fraction exceeds $3/8$: output "marked vertex exists"

**Why this works:** If there's a marked vertex $x_0$ at depth $\ell(x_0)$, construct the eigenvector:

$$|\phi\rangle = \sqrt{n}|r\rangle + \sum_{x \neq r,\, x \rightsquigarrow x_0} (-1)^{\ell(x)} |x\rangle$$

where $x \rightsquigarrow x_0$ means $x$ is on the path from root to $x_0$. One can verify $R_B R_A |\phi\rangle = |\phi\rangle$ (it's orthogonal to all diffusion states $|\psi_x\rangle$ for unmarked $x$), and $|\langle r | \phi\rangle / \|\phi\|| \geq 1/\sqrt{2}$. So phase estimation on $|r\rangle$ returns eigenvalue 1 with probability $\geq 1/2$.

If there's *no* marked vertex, the [[Effective Spectral Gap Lemma]] (Lee-Mittal-Reichardt-Špalek-Szegedy) bounds the weight of $|r\rangle$ on near-eigenvalue-1 components to $\|P_\chi |r\rangle\| \leq \chi\sqrt{Tn}$. Choosing $\chi = O(1/\sqrt{Tn})$ makes the false positive probability $< 1/4$.

**Complexity:** $O(\sqrt{Tn} \log(1/\delta))$ uses of $R_A$ and $R_B$.

### Search algorithm (finding a solution)

Use the detection algorithm as a subroutine in a binary search down the tree:

1. Run detection on the whole tree
2. If "exists": check each child subtree of the root, find one containing a marked vertex
3. Recurse into that subtree
4. Repeat until a marked leaf is reached

Since depth is $\leq n$ and degree is $O(1)$, this costs an extra factor of $O(n)$, giving total $O(\sqrt{T} n^{3/2} \log n)$.

### Unique solution variant

When promised a unique marked vertex $x_0$, a cleverer approach avoids binary search. The eigenvector $|\phi'\rangle = |\phi\rangle / \|\phi\|$ encodes the entire path from root to $x_0$ — measuring it (after phase estimation approximately produces $|\phi'\rangle$) gives a uniformly random vertex on that path, allowing a recursive halving strategy. The expected complexity is $O(\sqrt{Tn} \log^3 n)$.

---

## Key results

**Theorem 1 (Detection):** Given an upper bound $T$ on the backtracking tree size, there is a quantum algorithm that detects the existence of a solution using $O(\sqrt{Tn} \log(1/\delta))$ evaluations of $P$ and $h$, with failure probability $\leq \delta$.

**Theorem 2 (Search):** Without any promise on the number of solutions, there is a quantum algorithm finding a solution (or certifying none exists) using $O(\sqrt{T} n^{3/2} \log n \log(1/\delta))$ evaluations of $P$ and $h$. For a unique solution, this improves to $O(\sqrt{Tn} \log^3 n \log(1/\delta))$.

Both use $\text{poly}(n)$ space and $O(1)$ auxiliary operations per evaluation.

---

## Comparison with prior work

| Approach | Query complexity | Assumption |
|---|---|---|
| Grover search over $d^n$ assignments | $O(d^{n/2})$ | None |
| Cerf-Grover-Williams nested search | Depends on valid partial assignments at fixed depth | Distribution-dependent |
| Fürer's leaf search | $O(\sqrt{L} \cdot \text{poly}(n))$ | Need efficient leaf enumeration |
| Amplitude amplification of randomised alg. | $O(\text{poly}(n)/\sqrt{p})$ | Need randomised subroutine with success prob $p$ |
| **This paper (detection)** | $O(\sqrt{Tn})$ | Upper bound on $T$ |
| **This paper (search)** | $O(\sqrt{T} n^{3/2} \log n)$ | None |

The advantage over Grover is dramatic when backtracking prunes heavily: for a CSP where classical backtracking explores $T \ll d^n$ nodes, this gives $\sqrt{T} \cdot \text{poly}(n) \ll d^{n/2}$.

The advantage over amplitude amplification of randomised algorithms is that this works for *deterministic* backtracking algorithms, and the speedup is instance-dependent — fast classical instances remain fast quantumly.

---

## Average-case exponential speedup

An interesting secondary result: for the right distribution over CSP instances, the instance-dependent quadratic speedup can amplify to an exponential *average-case* separation. The argument uses Jensen's inequality. If $T_Q(X) \approx \sqrt{T_C(X)}$ and the classical runtime has a power-law tail $\Pr[T_C = t] \sim t^\beta$ with $-2 < \beta < -3/2$, then:

$$\mathbb{E}[T_Q] = \text{poly}(n), \qquad \mathbb{E}[T_C] = 2^{\Omega(n)}.$$

The paper demonstrates this concretely for random 3-SAT with a distribution over clause densities, achieving $\mathbb{E}[T_C] = \Omega(2^{0.094n})$ while $\mathbb{E}[T_Q] = \text{poly}(n)$.

This uses heavy-tailed distributions on backtracking tree sizes, which have some empirical support (Hogg-Williams, Jia-Moore) but remain debated. The paper is careful to note several caveats — the separation doesn't hold for randomised backtracking where the randomness is internal to the algorithm (rather than over instances).

---

## Limits / caveats

1. **Depth overhead is necessary.** A path of $T$ vertices has depth $T - 1$, and finding the marked endpoint requires $\Omega(T)$ queries. Aaronson-Ambainis showed $\Omega(\sqrt{Tn})$ is tight for detection in the worst case over all trees with $T$ nodes and depth $n$.

2. **Multiple marked vertices.** Finding one of $k > 1$ marked vertices doesn't give a $1/\sqrt{k}$ speedup, unlike Grover. Belov's argument: attach $k$ marked leaves below the unique target, find one in $o(\sqrt{Tn})$ to recover the target, contradicting the lower bound.

3. **Classical "lucky" search.** If the classical algorithm finds a solution early without exploring the whole tree, the quantum algorithm — which must explore the entire tree to detect the marked vertex — may not outperform it. The paper notes this as an open problem.

4. **Upper bound on $T$ needed for detection.** Without it, use exponential doubling (guessing $T = 1, 2, 4, \ldots$), which adds a factor of $O(n)$ to search.

5. **The speedup is "only" quadratic.** For CSPs on $n$ variables with classical runtime $2^{cn}$, the quantum algorithm runs in $2^{cn/2} \cdot \text{poly}(n)$. This is a constant-factor improvement in the exponent, not a change in scaling class.

---

## Reusable ideas

1. [[Quantum Walk on Backtracking Trees via Effective Resistance]] — The core construction: building a quantum walk on an implicitly-defined tree, with the walk starting at the root rather than at the stationary distribution. Belovs' connection between quantum walks and effective resistance, specialised to trees, gives the $\sqrt{Tn}$ detection bound. Unlike [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy walks]], the graph need not be known in advance.

2. [[Eigenvector-Encoded Path for Unique Search]] — When there's a unique marked vertex, the eigenvalue-1 eigenvector of the walk operator encodes the full root-to-target path. Measuring it yields a random vertex on this path, enabling a recursive halving strategy. This is a general trick for unique-target search on trees.

3. [[Average-Case Amplification via Power-Law Tails]] — Jensen's inequality applied to $T_Q \approx \sqrt{T_C}$ amplifies quadratic worst-case speedups to exponential average-case separations when the classical runtime has heavy tails. Applicable beyond backtracking to any setting with instance-dependent quadratic quantum speedups.

---

## References within this paper

- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — the Szegedy quantum walk framework. Montanaro's walk is related but doesn't need the stationary distribution start.
- Belovs, *Quantum walks and electric networks*, arXiv:1302.3143 (2013) — the main quantum subroutine. Relates quantum walk detection complexity to effective resistance.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — the baseline for unstructured quantum search.
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|Ambainis-Childs-Reichardt-Špalek-Zhang (2007)]] — uses a related quantum walk on trees for AND-OR formula evaluation.
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification, an alternative approach for randomised algorithms.
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|MNRS (2007)]] — unified quantum walk search framework; assumes known graph and stationary-distribution start.
- Lee-Mittal-Reichardt-Špalek-Szegedy, *Quantum query complexity of state conversion*, FOCS 2011 — the effective spectral gap lemma.
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation.
- Aaronson-Ambainis, *Quantum search of spatial regions*, Theory of Computing 1:47–79 (2005) — the $\Omega(\sqrt{Tn})$ lower bound.
- Cerf-Grover-Williams, *Nested quantum search*, PRA 61:032303 (2000) — an earlier, weaker attempt at quantum backtracking speedup.
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — uses Szegedy walks for Monte Carlo speedup; the same author's companion paper on general-purpose quadratic speedups.

---

## Cross-links

### Paper notes
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]]

### Trick cards
- [[Quantum Walk on Backtracking Trees via Effective Resistance]]
- [[Eigenvector-Encoded Path for Unique Search]]
- [[Average-Case Amplification via Power-Law Tails]]
