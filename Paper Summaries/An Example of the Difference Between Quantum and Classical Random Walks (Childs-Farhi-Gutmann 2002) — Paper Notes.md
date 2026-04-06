> **Source:** Andrew M. Childs, Edward Farhi, Sam Gutmann, *An example of the difference between quantum and classical random walks*, Quantum Information Processing **1**(1–2), 35–43 (2002); arXiv:quant-ph/0103020
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0103020) · [QIP](https://doi.org/10.1023/A:1019609420309)
> **Tags:** #quantum-walk #continuous-time #ballistic-transport #exponential-speedup #foundational

---

## The computational problem

**Graph traversal:** Given a specific family of graphs $G_n$ (two balanced binary trees of depth $n$ with leaves pairwise identified), start at the root of the left tree and reach the root of the right tree. The complexity measure is time: how long does it take for the walk to propagate from left root to right root?

This is not framed as a decision problem with an oracle — it's a direct comparison of propagation dynamics. The quantum advantage is in the *speed* of transport, not in query complexity per se.

---

## What the paper does

Defines continuous-time quantum walks on graphs and shows that on $G_n$, quantum propagation from left root to right root takes time $O(n)$ (linear, ballistic), while any classical random walk takes time $2^{\Omega(n)}$ (exponential). This is the first clean demonstration that continuous-time quantum walks can be exponentially faster than their classical analogues.

The paper also argues that continuous-time walks are more natural for general graphs than discrete-time walks, because discrete-time walks require an extra "coin" Hilbert space whose definition depends on global properties of the graph.

---

## The graph $G_n$

$G_n$ has $2^{n+1} + 2^n - 2$ vertices. Two balanced binary trees of depth $n$ are joined by identifying leaf $j$ of the left tree with leaf $j$ of the right tree. Vertices are grouped into columns $j \in \{0, 1, \ldots, 2n\}$:
- Column 0: left root (1 vertex)
- Column $j$ for $0 < j < n$: interior of left tree ($2^j$ vertices)
- Column $n$: shared leaves ($2^n$ vertices)
- Column $j$ for $n < j < 2n$: interior of right tree ($2^{2n-j}$ vertices)
- Column $2n$: right root (1 vertex)

---

## Classical analysis

On the left tree ($0 < j < n$), each vertex at column $j$ has 2 children (toward column $j+1$) and 1 parent (toward column $j-1$). A random walker from column $j$ steps toward the leaves with probability $2/3$ and toward the root with probability $1/3$. The walker drifts quickly toward the centre.

On the right tree ($n < j < 2n$), this reverses: probability $1/3$ to move toward the right root, $2/3$ to drift back toward the leaves. Starting at the centre, the walker faces an exponentially unfavourable drift. The probability of being at column $2n$ after any polynomial number of steps is less than $2^{-n}$.

---

## Quantum analysis

### Symmetry reduction

With the initial state $|\text{col } 0\rangle$ (left root), the symmetry of $H$ (the adjacency matrix as Hamiltonian, per equation (7)) confines evolution to the $(2n+1)$-dimensional subspace spanned by the column states:

$$
|\text{col } j\rangle = \frac{1}{\sqrt{N_j}} \sum_{a \in \text{column } j} |a\rangle
$$

where $N_j = 2^j$ for $j \le n$ and $N_j = 2^{2n-j}$ for $j \ge n$.

In this basis, the matrix elements are uniform:

$$
\langle \text{col } j | H | \text{col } j \pm 1\rangle = -\sqrt{2}\,\gamma
$$

$$
\langle \text{col } j | H | \text{col } j\rangle = \begin{cases} 2\gamma & j = 0, n, 2n \\ 3\gamma & \text{otherwise} \end{cases}
$$

The quantum walk on $G_n$ reduces to a quantum walk on a line of $2n+1$ vertices with uniform hopping $\sqrt{2}\gamma$ — the branching structure that creates the classical drift is completely invisible to the quantum dynamics.

### Ballistic propagation

On the translationally invariant infinite line with hopping $\sqrt{2}\gamma$, the amplitude to go from site $l$ to site $m$ in time $t$ is:

$$
\langle m | e^{-iHt} | l \rangle = e^{-i3\gamma t} \, i^{m-l} \, J_{m-l}(2\sqrt{2}\gamma t)
$$

where $J_{m-l}$ is a [[Bessel Linearisation of Quantum Walk Phases|Bessel function]] of order $m-l$. This gives propagation at speed $v = 2\sqrt{2}\gamma$: the wavepacket is exponentially small ahead of the wavefront $|m-l| > vt$, and has amplitude $O(|m-l|^{-1/2})$ near the front.

Since $G_n$ (reduced) is nearly identical to this infinite line for large $n$ (with a small "defect" at column $n$ from the different diagonal element), propagation from column 0 to column $2n$ occurs in time $T \approx 2n / (2\sqrt{2}\gamma) = O(n)$.

Numerical verification at $n = 500$, $\gamma = 1$ shows a clean wavepacket travelling at speed $2\sqrt{2}$, with a small reflection at the central defect but the leading edge essentially undisturbed.

---

## The limiting distribution

In the classical case, the limiting distribution $\pi_b = (2^{n+1} + 2^n - 2)^{-1}$ is uniform over all vertices — exponentially small at any particular vertex.

For the quantum walk, unitarity prevents convergence to a steady state. The time-averaged distribution is:

$$
\chi_b = \lim_{T \to \infty} \frac{1}{T} \int_0^T |\langle b | e^{-iHt} | a\rangle|^2 \, dt = \sum_r |\langle a | E_r\rangle|^2 \, |\langle b | E_r\rangle|^2
$$

By the left-right symmetry of $G_n$, $|\langle \text{col } 0 | E_r\rangle| = |\langle \text{col } 2n | E_r\rangle|$, and Cauchy-Schwarz gives:

$$
\chi_{\text{col } 2n} \ge \frac{1}{2n+1}
$$

So the time-averaged probability at the right root is $\Omega(1/n)$ — polynomially large, versus the classical $2^{-\Omega(n)}$.

---

## Key results

| Property | Classical | Quantum |
|---|---|---|
| Transport time (left root → right root) | $2^{\Omega(n)}$ | $O(n)$ |
| Limiting probability at right root | $(2^{n+1} + 2^n - 2)^{-1}$ | $\ge (2n+1)^{-1}$ |
| Propagation mode | Diffusive (biased against right) | Ballistic (speed $2\sqrt{2}\gamma$) |

---

## Comparison with prior work

| Paper | What it showed | Graph |
|---|---|---|
| [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes\|Childs et al. (2003)]] | Exponential separation with oracle | Glued trees with random cycle (not simple identification) |
| This paper (2002) | Exponential transport speedup, no oracle | $G_n$ (simple leaf identification) |

This paper came first (2001 preprint, 2002 publication) and demonstrated the phenomenon on the simpler graph $G_n$. The 2003 paper hardened this into a computational separation by randomising the leaf connections to prevent classical backtracking from the middle. The classical $O(n^2)$ backtracking strategy that works on $G_n$ (you can detect the central column by vertex degree) is precisely what the random alternating cycle in the 2003 construction defeats.

---

## Limits / caveats

- The simple graph $G_n$ has a structural weakness: a classical algorithm can detect column $n$ by vertex degree (valence pattern changes there) and use this to traverse in $O(n^2)$ time via backtracking from the identified centre. This is why the 2003 paper needed to randomise the connections.
- The Hamiltonian uses the adjacency matrix directly, requiring knowledge of the hopping rate $\gamma$, but $\gamma$ is a free parameter that doesn't depend on global graph properties.
- The analysis is specific to this graph family. Extending quantum walk speedups to broader graph classes required the machinery of [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs-Goldstone (2004)]], [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|Magniez-Nayak-Roland-Santha (2007)]], and [[Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016) — Paper Notes|Krovi-Magniez-Ozols-Roland (2016)]].
- The continuous-time framework avoids the "coin" issue of discrete-time walks, but [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] later showed the two formulations are equivalent via Szegedy quantization.

---

## Reusable ideas

1. **[[Symmetry Reduction to Column Subspace in Quantum Walks]]** — The branching structure of the tree becomes invisible after projecting onto the column subspace. What looks like an asymmetric graph with exponential classical drift becomes a uniform line. This reduction is the key reason the quantum walk doesn't "feel" the classical drift.

2. **[[Ballistic Quantum Transport via Bessel Functions]]** — On a uniform line, quantum propagation is ballistic (linear in $t$) with amplitude $J_{m-l}(vt)$, versus classical diffusive $O(\sqrt{t})$. The Bessel function structure gives both the wavefront speed and the $O(t^{-1/2})$ amplitude scaling at the front.

---

## References within this paper

- Farhi and Gutmann, *Quantum computation and decision trees*, Phys. Rev. A **58**, 915 (1998) — original continuous-time quantum walk paper and framework for decision problems
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs, Cleve, Deotto, Farhi, Gutmann, Spielman (2003)]] — the oracle-based exponential separation that built on this work
- Aharonov, Ambainis, Kempe, Vazirani, *Quantum walks on graphs*, quant-ph/0012090 — discrete-time quantum walks and mixing times
- Aharonov, Davidovich, Zagury (1993) — coined quantum random walks
- Nayak and Vishwanath, quant-ph/0010117 — quantum walk on the line (discrete-time)
- Meyer (1996) — impossibility of coinless discrete-time walks in 1D

---

## Cross-links

### Paper notes
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — the strengthened oracle version of this result
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]] — continuous-time walk search on lattices, extending the transport idea to search
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]] — unifies the continuous/discrete frameworks used here
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]] — quantum property testing using quantum walk techniques
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]] — survey of quantum walk algorithms

### Trick cards
- [[Symmetry Reduction to Column Subspace in Quantum Walks]] — new (this batch)
- [[Ballistic Quantum Transport via Bessel Functions]] — new (this batch)
- [[Continuous-Time Quantum Walk Search]] — the search generalisation
- [[Bessel Linearisation of Quantum Walk Phases]] — related Bessel function structure
