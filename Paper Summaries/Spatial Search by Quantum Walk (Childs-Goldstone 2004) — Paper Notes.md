> **Source:** Andrew M. Childs and Jeffrey Goldstone, *Spatial search by quantum walk*, Phys. Rev. A **70**, 022314 (2004); arXiv:quant-ph/0306054
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0306054) · [PRA](https://doi.org/10.1103/PhysRevA.70.022314)
> **Tags:** #quantum-walk #search #spatial-search #Grover #lattice #continuous-time

---

## The problem

**Spatial search:** $N$ items are stored at vertices of a $d$-dimensional lattice. A "quantum robot" can move along edges in superposition. Find a marked vertex $w$.

The challenge: Grover's algorithm requires all-to-all connectivity (each step flips the sign of *any* marked element). On a lattice, each step can only move to adjacent vertices, costing $O(N^{1/d})$ per Grover iteration. Naively, the total time is $\sqrt{N} \cdot N^{1/d} = N^{1/2 + 1/d}$ — **no speedup** in $d = 2$.

**This paper:** Uses a continuous-time quantum walk to achieve $O(\sqrt{N})$ search time for $d > 4$, $O(\sqrt{N}\,\text{polylog}\,N)$ for $d = 4$, and shows the algorithm fails for $d < 4$.

---

## What the paper does

Defines a continuous-time quantum walk search algorithm on arbitrary graphs and analyses it exactly on $d$-dimensional periodic lattices. The Hamiltonian is:

$$
H = -\gamma L + |w\rangle\langle w|
$$

where $L = A - D$ is the graph Laplacian ($A$ = adjacency matrix, $D$ = degree matrix), $\gamma > 0$ is the hopping rate, and $|w\rangle\langle w|$ is the oracle marking the target vertex.

Start in $|s\rangle = N^{-1/2}\sum_j |j\rangle$ (uniform superposition). Evolve under $H$ for time $T$. Measure. The algorithm works when the evolved state $e^{-iHT}|s\rangle$ has large overlap with $|w\rangle$.

---

## The spectral argument (why the algorithm works)

The analysis reduces to a 2D subspace spanned by $|s\rangle$ and $|w\rangle$:

1. **$|s\rangle$ is the ground state of $-\gamma L$** (eigenvalue 0)
2. **As $\gamma$ varies**, the ground state of $H$ transitions from being close to $|w\rangle$ (small $\gamma$) to close to $|s\rangle$ (large $\gamma$)
3. **At the critical $\gamma$**, the ground state and first excited state both have $O(1)$ overlap with $|s\rangle$ and $|w\rangle$
4. **The energy gap** $E_1 - E_0$ determines the rotation time: $T = O(1/(E_1 - E_0))$

This is essentially an **avoided level crossing** — the same mechanism as [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|adiabatic computation]], but here we run at the critical point for a fixed time instead of sweeping slowly.

### The eigenvalue condition

Using the lattice eigenstates $|\phi(k)\rangle$, the eigenvalue condition reduces to:

$$
F(E) = \frac{1}{N}\sum_k \frac{1}{\gamma E(k) - E} = 1
$$

where $E(k) = 2(d - \sum_j \cos k_j)$ are the Laplacian eigenvalues. The ground and first excited state energies are the smallest solutions of this equation.

---

## Results by dimension

| Dimension $d$ | Critical $\gamma$ | Gap at critical | Success prob. | Time | Speedup? |
|---|---|---|---|---|---|
| Complete graph ($d \sim N$) | $1/N$ | $\Theta(1/\sqrt{N})$ | $\Theta(1)$ | $O(\sqrt{N})$ | Full (= Grover) |
| Hypercube ($d = \log N$) | $\frac{2}{n} + O(n^{-2})$ | $\Theta(1/\sqrt{N})$ | $\Theta(1)$ | $O(\sqrt{N})$ | Full |
| $d > 4$ | $I_{1,d}$ | $\Theta(1/\sqrt{N})$ | $\Theta(1)$ | $O(\sqrt{N})$ | **Full** |
| $d = 4$ | $I_{1,4}$ | $\Theta(1/\sqrt{N\log N})$ | $O(1/\log N)$ | $O(\sqrt{N\log N})$ | Near-full |
| $d = 3$ | $I_{1,3}$ | $O(N^{-2/3})$ | $O(N^{-1/3})$ | — | $O(N^{2/3})$ total, **not full** |
| $d = 2$ | $\frac{\ln N}{4\pi} + A$ | $O(N^{-1}\ln N)$ | $O((\ln N)^2/N)$ | — | Essentially **none** |

where $I_{j,d} = \frac{1}{(2\pi)^d}\int \frac{d^dk}{[E(k)]^j}$ (converges for $d > 2j$).

### The critical dimension

The dimension $d = 4$ is the **upper critical dimension** — a phase-transition concept from statistical mechanics. The key quantities are the lattice sums $S_{j,d}$:

$$
S_{j,d} = \frac{1}{N}\sum_{k \neq 0} \frac{1}{[E(k)]^j}
$$

For $d > 2j$: $S_{j,d} = I_{j,d} + o(1)$ (converges to a constant). For $d < 2j$: $S_{j,d} \sim c_{j,d} N^{2j/d - 1}$ (diverges with $N$). For $d = 2j$: $S_{j,d} \sim \frac{\ln N}{(4\pi)^j j!}$ (logarithmic divergence).

The algorithm needs $S_{2,d}$ to be $O(1)$ for the gap analysis to give $\Theta(1/\sqrt{N})$. This requires $d > 2 \times 2 = 4$. At $d = 4$: logarithmic corrections.

---

## Connection to other approaches

| Approach | Time | Notes |
|---|---|---|
| Grover (all-to-all) | $O(\sqrt{N})$ | Requires non-local oracle |
| Aaronson-[[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] | $O(\sqrt{N})$ for $d > 2$; $O(\sqrt{N}\,\text{polylog})$ for $d = 2$ | Divide and conquer |
| **Childs-Goldstone (this paper)** | $O(\sqrt{N})$ for $d > 4$ | Continuous-time walk on lattice |
| Ambainis-Kempe-Rivosh (2005) | $O(\sqrt{N\log N})$ for $d = 2$ | Coined discrete walk |

The Aaronson-Ambainis result is stronger in $d = 2, 3$ because they use a recursive divide-and-conquer strategy rather than a single walk. The continuous-time walk approach is more natural and analytically clean, but misses the low-dimensional case.

---

## The complete graph as Grover

On the complete graph, $L + NI = N|s\rangle\langle s|$, so:

$$
H = -\gamma(N|s\rangle\langle s| - NI) + |w\rangle\langle w| = \gamma N I - \gamma N|s\rangle\langle s| + |w\rangle\langle w|
$$

Dropping the scalar $\gamma N I$, this is the Farhi-Gutmann "analog analogue" of Grover: the Hamiltonian drives oscillations between $|s\rangle$ and $|w\rangle$ at rate $1/\sqrt{N}$ when $\gamma = 1/N$. The complete graph = infinite dimension, so it trivially satisfies $d > 4$.

---

## Reusable ideas

1. **[[Continuous-Time Quantum Walk Search]]:** Search by evolving under $H = -\gamma L + |w\rangle\langle w|$ starting from $|s\rangle$. At a critical hopping rate $\gamma$, the ground and first excited states mix $|s\rangle$ and $|w\rangle$, and the system oscillates between them in time $O(1/\Delta E)$. A natural alternative to Grover for spatially structured problems.

2. **[[Avoided Crossing at Critical Hopping Rate]]:** The search algorithm works at a specific critical $\gamma$ where the ground and first excited states undergo an avoided level crossing between $|s\rangle$ and $|w\rangle$. Away from this critical value, $|s\rangle$ is approximately an eigenstate and the algorithm fails. The gap at the critical point determines the speedup, and the dimension determines whether the gap closes fast enough.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — the all-to-all quantum search; this paper's complete graph case
- Aaronson & [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — spatial search via divide-and-conquer; achieves $\sqrt{N}$ for $d > 2$
- Farhi & Gutmann (1998) — continuous-time quantum walk on the complete graph ("analog analogue of Grover")
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Boyer, Brassard, Høyer & Tapp (1998)]] — tight bounds on quantum search
- Benioff (2002) — quantum robot model (spatial quantum computation)
- Ambainis, Kempe & Rivosh (2005) — coined quantum walk search on 2D lattice
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — survey placing spatial search in the broader quantum walk framework

---

## Cross-links

### Paper notes
- [[Universal Computation by Quantum Walk (Childs 2009) — Paper Notes]] — later universality result for continuous-time quantum walk; this paper is one of the main algorithmic precedents
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — the standard (non-spatial) amplitude amplification framework
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — discrete-time walk framework for search; complementary approach
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]] — another application of structured quantum search

### Trick cards
- [[Continuous-Time Quantum Walk Search]]
- [[Avoided Crossing at Critical Hopping Rate]]
- [[Standard Amplitude Amplification]] — the non-spatial alternative
- [[Superposition Query for Global Properties]]
