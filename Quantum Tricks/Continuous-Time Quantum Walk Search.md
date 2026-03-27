
> **Source:** Childs & Goldstone, arXiv:quant-ph/0306054 (2004); Farhi & Gutmann (1998) for complete graph case
> **Tags:** #trick #quantum-walk #search #spatial-search #Hamiltonian

## What it does

Searches for a marked vertex $w$ on a graph $G$ by evolving under the Hamiltonian $H = -\gamma L + |w\rangle\langle w|$, where $L$ is the graph Laplacian and $\gamma$ is a tunable hopping rate. Starting from the uniform superposition $|s\rangle$, the system oscillates into $|w\rangle$ in time determined by the spectral gap.

## The trick

1. Define $H = -\gamma L + |w\rangle\langle w|$ on the $N$-vertex graph
2. Start in $|s\rangle = N^{-1/2}\sum_j |j\rangle$
3. Tune $\gamma$ to the **critical value** where the ground and first excited states undergo an [[Avoided Crossing at Critical Hopping Rate|avoided crossing]]
4. Evolve for time $T = O(1/\Delta E)$ where $\Delta E = E_1 - E_0$ is the gap
5. Measure — find $|w\rangle$ with $O(1)$ probability (in high enough dimension)

**Complete graph:** $\gamma = 1/N$, gap $= 2/\sqrt{N}$, time $= O(\sqrt{N})$ — recovers the "analog Grover" of Farhi & Gutmann.

**$d$-dimensional lattice:**

| $d$ | Gap at critical $\gamma$ | Time | Success |
|---|---|---|---|
| $> 4$ | $\Theta(1/\sqrt{N})$ | $O(\sqrt{N})$ | $\Theta(1)$ |
| $= 4$ | $\Theta(1/\sqrt{N\log N})$ | $O(\sqrt{N\log N})$ | $O(1/\log N)$ |
| $< 4$ | Too small | — | Fails |

## When to reach for it

- Spatial search on graphs where the marked vertex is local (can't use all-to-all oracle)
- When the problem has natural graph structure (lattice, expander, etc.)
- Understanding the role of connectivity/dimension in quantum speedups
- Complementary to [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|discrete-time walk]] approaches (Szegedy, Ambainis)

## Complexity

Time: $O(\sqrt{N})$ for $d > 4$ (same as Grover). The algorithm uses **continuous** Hamiltonian evolution — no discrete oracle calls. The oracle is encoded in the $|w\rangle\langle w|$ term.

## Caveat

Fails for $d < 4$ — the spectral gap at the critical point is too small. In $d = 2$, the success probability is only $O((\log N)^2/N)$, giving no speedup. The coined discrete-time walk (Ambainis-Kempe-Rivosh 2005) achieves $O(\sqrt{N\log N})$ in $d = 2$, beating the continuous-time approach.

Requires knowing the critical $\gamma$ — for lattices it's $I_{1,d}$ (a lattice-dependent constant). For general graphs, finding the critical $\gamma$ may require spectral information about $L$.

## Related notes

- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]]
- [[Avoided Crossing at Critical Hopping Rate]]
- [[Standard Amplitude Amplification]] — the non-spatial alternative
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
