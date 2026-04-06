# Momentum Filtering on Graphs

> **Source:** Andrew M. Childs, arXiv:0806.1972
> **Tags:** #trick #quantum-walk #scattering #filtering #spectral

## What it does

Projects a quantum walk wave packet onto a narrow momentum band by passing it through a sequence of small graph substructures (filter widgets) that exponentially suppress unwanted momentum components.

## The trick

On a line graph, a quantum walk has momentum eigenstates $|\tilde{k}\rangle$ with $\langle x|\tilde{k}\rangle = e^{ikx}$ and eigenvalues $2\cos k$. When you attach a small subgraph to the line, it acts as a momentum-dependent filter: only certain momenta transmit through perfectly.

**Filter widget:** A specific small graph (Fig. 1d in the paper) has a transmission coefficient where $|T(k)|^2$ is sharply peaked around $k = -\pi/4$ and $k = -3\pi/4$, with exponential suppression elsewhere. Each filter stage is degree 3 and adds effective length 2 at $k = -\pi/4$.

**Cascading:** $m_d = \log\Theta(m^2)$ filter widgets in series suppress unwanted momenta to exponentially small amplitude. The transmission of the cascade at $k = -\pi/4$ remains perfect (since each individual filter has $|T(-\pi/4)| = 1$).

**Momentum separator:** A second widget type (Fig. 1e) has perfect transmission at both $k = -\pi/4$ and $k = -3\pi/4$ but with very different effective lengths ($\ell \approx 0.686$ vs $\ell \approx 23.3$). This temporally separates the two momentum components even though they propagate at the same group velocity $|v| = \sqrt{2}$.

The separator breaks the bipartite symmetry of the graph ($k \to -\pi - k$ symmetry), which is what allows it to distinguish the two momenta.

## When to reach for it

- Any quantum walk construction where the computation works at a specific momentum but the initial state has broad momentum support
- Analogous to optical bandpass filtering, but in the graph/walk setting
- When you need to suppress dispersive errors in scattering-based quantum walk algorithms

## Complexity

$O(\log m)$ filter widgets for an $m$-gate computation. Each widget adds $O(1)$ vertices and $O(1)$ effective length to the wire.

## Caveat

The filter transmits both $k = -\pi/4$ and $k = -3\pi/4$, requiring the additional momentum separator. The separator has different effective lengths at the two momenta but same group velocity — the temporal separation requires wires long enough to resolve the arrival time difference.

## Related notes
- [[Universal Computation by Quantum Walk (Childs 2009) — Paper Notes]]
- [[Gate Implementation via Graph Scattering]]
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]]
