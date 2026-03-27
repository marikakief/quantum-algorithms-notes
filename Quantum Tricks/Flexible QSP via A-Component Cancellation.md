# Flexible QSP via A-Component Cancellation

> **Source:** Low & Chuang, arXiv:1707.05391  
> **Tags:** #trick #QSP #block-encoding #quantum-signal-processing

## What it does

Drops the unintuitive constraints (4) and (5) from the original [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] achievability conditions (Lemma 1 of the qubitization paper). Any bounded polynomial $B(x)$ with $B(0) = 0$ and correct parity can now be implemented exactly as a standard-form encoding, using one extra ancilla qubit.

## The trick

The composite qubiterate $\hat{W}_{\vec{\phi}}$ encodes $A[\hat{H}] + iB[\hat{H}]$ in standard-form. To isolate $B[\hat{H}]$ alone, take a superposition:

1. Prepare ancilla $|+\rangle_c$ and apply $\hat{W}_{\vec{\phi}}$ (controlled on $|0\rangle_c$) and $\hat{W}_{-\vec{\phi}}$ (controlled on $|1\rangle_c$).
2. By the symmetry $\hat{W}_{-\vec{\phi}} = A\hat{I} - iB\hat{\sigma}_z + iC\hat{\sigma}_x - iD\hat{\sigma}_y$, the $A$ component is the same in both branches and the $B$ component flips sign.
3. Projecting onto $|+\rangle_c|0\rangle_{ab}$ yields $B[\hat{H}]$ — the $A$ terms cancel exactly.

The result encodes $B[\hat{H}]$ in standard-form-$(B[\hat{H}], 1, \hat{V}_{\vec{\phi}}, 4d)$. The achievable conditions on $B$ are just:
- Real parity-$(N \bmod 2)$ polynomial of degree $\leq N$
- $B(0) = 0$
- $|B(x)| \leq 1$ for $x \in [-1, 1]$

No constraints on behaviour outside $[-1,1]$ — those were the conditions that made the original QSP hard to use in practice.

## When to reach for it

- Any time you want to implement a polynomial transformation of a [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoded]] operator and the polynomial doesn't naturally satisfy the "outside-the-interval" conditions of the original QSP achievability lemma.
- Designing spectral filters, eigenvalue transformations, or operator approximations where $B(0) = 0$ is natural.
- The $B(0) = 0$ constraint means this naturally handles functions that vanish at the origin — good for spectral amplification, bad for constant shifts.

## Complexity

$O(N)$ queries to controlled-$\hat{U}$, $O(N\log d)$ primitive gates, 1 extra ancilla qubit (total: 3 ancilla qubits including the QSP workspace). Same query complexity as standard [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]].

## Caveat

You can only implement $B[\hat{H}]$, not $A[\hat{H}] + iB[\hat{H}]$. The cancellation discards the $A$ component entirely, so the unitary is not "fully coherent" in the sense of [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes|Martyn et al. (2023)]]. Also, $B(0) = 0$ is required — you can't implement a constant function this way.

## Related notes
- [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Amplitude Multiplication (Linear Rescaling of Unknown Amplitudes)]]
- [[Phased Qubitization Sequence]]
- [[Signal Transduction via Controlled-W]]
