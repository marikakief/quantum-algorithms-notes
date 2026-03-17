
> **Source:** Andrew M. Childs and Robin Kothari, *Limitations on the simulation of non-sparse Hamiltonians*, arXiv:0908.4398, Quantum Information and Computation **10**, 669–684 (2010)  
> **Links:** [arXiv](https://arxiv.org/abs/0908.4398) · [QIC](https://doi.org/10.26421/QIC10.7-8)  
> **Tags:** #hamiltonian-simulation #lower-bounds #black-box #non-sparse  
> **⚠ Author note:** The file title includes "Somma" but this paper has two authors: Childs and Kothari. Somma is not an author.

---

## What the paper does

An obstruction paper. It shows that the dream of simulating an arbitrary dense Hamiltonian in the entry-oracle model with complexity depending only on $\|H\|t$ and $\log N$ is not achievable, and explains why the walk-based simulation of Berry–Childs (0910.4157) is not dramatically improvable in general.

Two main limitations, plus one positive result.

## The norm-gap problem

The quantum-walk simulation of Berry–Childs scales with $\|\text{abs}(H)\|$ (spectral norm of the entrywise-absolute-value matrix), not $\|H\|$ itself. These two norms satisfy

$$
\|H\| \le \|\text{abs}(H)\| \le \|H\|_1 \le \sqrt{N}\,\|H\|,
$$

and the Hadamard tensor $H = R^{\otimes \log N}$ (where $R = \begin{pmatrix}1&1\\1&-1\end{pmatrix}/\sqrt{2}$) achieves $\|H\| = 1$ but $\|\text{abs}(H)\| = \sqrt{N}$. So for some Hamiltonians, the walk-based simulation incurs an $\sqrt{N}$ overhead over what $\|H\|t$ alone would suggest.

The paper shows this is not just a proof gap: it is an intrinsic oracle-model limitation.

## Two lower bounds

**Theorem (no-fast-forwarding for dense Hamiltonians):** Generalizes the Berry–Ahokas–Cleve–Sanders no-fast-forwarding result from sparse to dense Hamiltonians. Even though $\|H\|$ is not a unique measure of simulation difficulty for non-sparse $H$, any simulation using $o(\|H\|t)$ queries is impossible for some entry-oracle Hamiltonian.

**Stronger bound (poly-time obstruction):** There exist dense Hamiltonians for which simulation using $\text{poly}(\|H\|t, \log N)$ queries is impossible. Specifically, this rules out the possibility that the walk-based complexity $O(\|\text{abs}(H)t\|/\sqrt{\delta})$ can be dramatically improved to something polynomial in $\|H\|t$ and $\log N$ alone.

The construction uses circulant Hamiltonians (diagonalized by the DFT), whose eigenphases encode Boolean functions. Simulating such a Hamiltonian and then measuring in the Fourier basis solves a hard oracle problem, giving query lower bounds via reductions from parity or distinguishability.

## Positive result

Some non-sparse Hamiltonians can be simulated efficiently. In particular, Hamiltonians whose interaction graph has small **arboricity** $a$ (roughly: can be decomposed into $a$ forests) admit simulation with complexity depending on $a$ rather than $N$. On tree-structured graphs, a diagonal gauge transformation (choosing node phases along root-to-leaf paths) can make all off-diagonal entries nonnegative, reducing simulation to the $\|\text{abs}(H)\| = \|H\|$ case. This gives efficient simulation when the gauge-cleared norm is small.

## The lesson

The problem is not that dense simulation is impossible — it is that the **entry-oracle model** is the wrong interface. It exposes entrywise magnitudes (informing $\|\text{abs}(H)\|$) but not the phase cancellations that make $\|H\| \ll \|\text{abs}(H)\|$. Later advances in simulation (LCU, block-encoding, qubitization, QSVT) use access models that explicitly encode the structure making $H$ simulable, bypassing this norm-gap obstacle.

## Norms involved

| Norm | Definition | Relation |
|---|---|---|
| Max norm $\|H\|_{\max}$ | $\max_{jk} |H_{jk}|$ | smallest |
| Max column norm $\text{mcn}(H)$ | $\max_j \|He_j\|_2$ | |
| Spectral norm $\|H\|$ | $\max_{\|v\|=1}\|Hv\|$ | |
| $\|\text{abs}(H)\|$ | spectral norm of $|H_{jk}|$ | $\ge \|H\|$ |
| Induced 1-norm $\|H\|_1$ | $\max_j \sum_k |H_{jk}|$ | $\le \sqrt{N}\,\text{mcn}(H)$ |

All inequalities in the chain $\|H\|_{\max} \le \text{mcn}(H) \le \|H\| \le \|\text{abs}(H)\| \le \|H\|_1 \le \sqrt{N}\,\text{mcn}(H)$ are tight.

## References within this paper

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — sparse simulation; this paper asks what happens without sparsity
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry & Childs (2012)]] — quantum walk simulation (sparse case)
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov & Ta-Shma (2003)]] — first sparse Hamiltonian simulation
- Childs (2010, Commun. Math. Phys. 294) — quantum walk approach achieving linear-in-$d$ scaling

---

## Cross-links

- [[Norm-Gap Obstruction between H and abs(H) in Oracle Simulation]]
- [[Hardness Embedding via Circulant Hamiltonian Eigenphases]]
- [[Diagonal Gauge Phase Removal on Tree-Structured Hamiltonians]]
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
