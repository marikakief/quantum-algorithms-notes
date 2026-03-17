# Avoided Crossing at Critical Hopping Rate

> **Source:** Childs & Goldstone, arXiv:quant-ph/0306054 (2004), §IV.B–D
> **Tags:** #trick #spectral #quantum-walk #phase-transition #search

## What it does

In the continuous-time quantum walk search Hamiltonian $H = -\gamma L + |w\rangle\langle w|$, there is a critical hopping rate $\gamma_c$ where the ground and first excited states undergo an avoided level crossing. The marked state $|w\rangle$ transitions from being the ground state (small $\gamma$) to being essentially invisible (large $\gamma$). At $\gamma = \gamma_c$, both low-energy eigenstates have $O(1)$ overlap with $|s\rangle$ and $|w\rangle$, enabling the search.

## The mechanism

- For $\gamma \gg \gamma_c$: ground state $\approx |s\rangle$ (the Laplacian dominates; the oracle perturbation is negligible). The algorithm fails — $|s\rangle$ is nearly an eigenstate, so time evolution doesn't change it.
- For $\gamma \ll \gamma_c$: ground state $\approx |w\rangle$ (the oracle dominates). First excited state $\approx |s\rangle$. Again fails — the two states don't mix.
- At $\gamma = \gamma_c$: the ground and first excited states are $\approx \frac{1}{\sqrt{2}}(|w\rangle \pm |s\rangle)$. The gap between them determines the oscillation period $T = \pi/(E_1 - E_0)$.

**On a $d$-dimensional lattice:** $\gamma_c = I_{1,d}$ (a lattice Green's function integral). The gap at $\gamma_c$ is $\Theta(1/\sqrt{N})$ for $d > 4$, giving $T = O(\sqrt{N})$.

## The connection to phase transitions

The switching of the ground state at $\gamma_c$ is a **quantum phase transition** in the walk Hamiltonian. The critical dimension $d = 4$ is the **upper critical dimension** — below which the gap doesn't scale as $1/\sqrt{N}$ and mean-field behaviour breaks down. This is the same critical dimension as in $\phi^4$ field theory, because the lattice Green's function sum $S_{2,d} = \frac{1}{N}\sum_k [E(k)]^{-2}$ diverges for $d \leq 4$.

## When to reach for it

- [[Continuous-Time Quantum Walk Search|Continuous-time walk search]]: must tune $\gamma$ to $\gamma_c$ for the algorithm to work
- Understanding why quantum walk search has a dimensional threshold
- Drawing connections between quantum algorithms and statistical mechanics (critical exponents, mean-field theory)
- Any quantum algorithm based on avoided crossings (cf. [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|adiabatic computation]])

## Caveat

Away from $\gamma_c$, the success probability is at most $O(1/N)$ regardless of evolution time — the algorithm completely fails. The width of the "good" $\gamma$ window is $O(1/\sqrt{N})$ around $\gamma_c$ for $d > 4$.

For $d < 4$: even at $\gamma_c$, the overlap of $|w\rangle$ on the low-energy eigenstates vanishes with $N$, and the success probability is too small for $\sqrt{N}$ speedup.

## Related notes

- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]]
- [[Continuous-Time Quantum Walk Search]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]] — adiabatic algorithm also relies on avoided crossings
- [[Standard Amplitude Amplification]] — Grover-style alternative without graph structure
