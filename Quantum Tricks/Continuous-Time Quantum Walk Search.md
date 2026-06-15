
> **Source:** Childs & Goldstone, arXiv:quant-ph/0306054 (2004); Farhi & Gutmann (1998) for complete graph case
> **Tags:** #trick #quantum-walk #search #spatial-search #Hamiltonian

## What it does

Searches for a marked vertex $w$ on a graph $G$ by evolving under the Hamiltonian $H = -\gamma L + |w\rangle\langle w|$, where Childs-Goldstone use $L=A-D$ (the negative of the standard graph Laplacian $D-A$) and $\gamma$ is a tunable hopping rate. Starting from the uniform superposition $|s\rangle$, the system can rotate toward $|w\rangle$ in time determined by the relevant low-energy gap.

## The trick

1. Define $H = -\gamma L + |w\rangle\langle w|$ on the $N$-vertex graph: graph-local hopping plus a continuous-time oracle term for the marked vertex
2. Start in $|s\rangle = N^{-1/2}\sum_j |j\rangle$
3. Tune $\gamma$ to the **critical value** where the marked perturbation creates an [[Avoided Crossing at Critical Hopping Rate|avoided-crossing-like]] two-level effect
4. Evolve for time $T = O(1/\Delta E)$ where $\Delta E = E_1 - E_0$ is the gap
5. Measure — find $|w\rangle$ with $O(1)$ probability (in high enough dimension)

**Complete graph:** $\gamma = 1/N$, gap $= 2/\sqrt{N}$, time $= O(\sqrt{N})$ — recovers the "analog Grover" of Farhi & Gutmann.

**$d$-dimensional lattice:**

| $d$ | Gap at critical $\gamma$ | Raw evolution time | Success after raw evolution |
|---|---|---|---|
| $> 4$ | $\Theta(1/\sqrt{N})$ | $O(\sqrt{N})$ | $\Theta(1)$ |
| $= 4$ | $\Theta(1/\sqrt{N\log N})$ | $O(\sqrt{N\log N})$ | $O(1/\log N)$ |
| $< 4$ | Too small / overlap too small | Not useful by itself | Fails as a constant-success search method |

## When to reach for it

- Spatial search on graphs where the marked vertex is local (can't use all-to-all oracle)
- When the problem has the lattice structure analysed by Childs-Goldstone, or another graph family where the corresponding spectral sums can be controlled
- Understanding the role of connectivity/dimension in quantum speedups
- Complementary to [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|discrete-time walk]] approaches (Szegedy, Ambainis)

## Complexity

Time: $O(\sqrt{N})$ for $d > 4$ with constant success (same scaling as Grover). In $d=4$, the raw evolution time is $O(\sqrt{N\log N})$ but the success probability is only $O(1/\log N)$, so constant-success search needs extra repetition or amplification. The algorithm uses **continuous** Hamiltonian evolution; its oracle is the $|w\rangle\langle w|$ term, not a coined/discrete update oracle.

## Caveat

Fails for $d < 4$ — the spectral gap and marked-state overlap at the critical point are too small. In $d = 2$, the success probability is only $O((\log N)^2/N)$, giving no speedup. The coined discrete-time walk (Ambainis-Kempe-Rivosh 2005) uses different dynamics and achieves $O(\sqrt{N\log N})$ in $d = 2$, beating this continuous-time approach.

Requires knowing the critical $\gamma$ — for lattices it's $I_{1,d}$ (a lattice-dependent constant). For general graphs, finding the critical $\gamma$ may require spectral information about $L$.

## Related notes

- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]]
- [[Avoided Crossing at Critical Hopping Rate]]
- [[Standard Amplitude Amplification]] — the non-spatial alternative
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
