# Ballistic Quantum Transport via Bessel Functions

> **Source:** Childs, Farhi, Gutmann, arXiv:quant-ph/0103020 (2002)
> **Tags:** #trick #quantum-walk #ballistic-transport #Bessel-function

## What it does

On a uniform infinite line with hopping rate $\sqrt{2}\gamma$, the quantum walk amplitude from site $l$ to site $m$ at time $t$ is given exactly by a Bessel function, yielding ballistic propagation at speed $v = 2\sqrt{2}\gamma$ — qualitatively different from the $O(\sqrt{t})$ diffusive spread of a classical random walk.

## The trick

The transition amplitude on a translationally invariant line with hopping $\sqrt{2}\gamma$ and diagonal $3\gamma$ is:

$$
\langle m | e^{-iHt} | l \rangle = e^{-i3\gamma t} \, i^{m-l} \, J_{m-l}(2\sqrt{2}\gamma t)
$$

where $J_k$ is a Bessel function of the first kind, order $k$.

Key properties of this propagator:
- **Wavefront speed:** $v = 2\sqrt{2}\gamma$. For $|m-l| \gg 1$ and $t < (v^{-1} - \varepsilon)|m-l|$, the amplitude is exponentially small.
- **Near-front amplitude:** $O(|m-l|^{-1/2})$ at the wavefront, giving probability $O(1/|m-l|)$.
- **Ballistic vs. diffusive:** Position spreads linearly with time, not as $\sqrt{t}$.

For a finite line of length $2n+1$ (as in the [[Symmetry Reduction to Column Subspace in Quantum Walks|column-reduced]] glued trees), the propagation approximates the infinite-line result until the wavepacket hits the boundary, where it reflects.

## When to reach for it

- Analysing transport on any graph whose symmetry-reduced form is (approximately) a uniform line
- Establishing that quantum walk transport is $O(n)$ on a path of length $n$, versus classical $O(n^2)$
- Any setting where you need the exact propagator for a nearest-neighbour hopping Hamiltonian on $\mathbb{Z}$

## Complexity

The propagation time to traverse a line of length $L$ is $T = O(L/\gamma)$. Combined with [[Symmetry Reduction to Column Subspace in Quantum Walks]], this gives $O(n)$ traversal time on glued trees with $2^{O(n)}$ vertices.

## Caveat

The exact Bessel form requires translational invariance. Small defects (like the different diagonal at column $n$ in glued trees) cause partial reflections but don't destroy ballistic transport. Larger inhomogeneities or disorder can localise the walk (cf. Anderson localisation), breaking ballistic transport entirely.

## Related notes
- [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes]]
- [[Symmetry Reduction to Column Subspace in Quantum Walks]]
- [[Bessel Linearisation of Quantum Walk Phases]]
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]]
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]]
