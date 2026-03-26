# Sparse Altered Hamiltonians via Gaussian Perturbation

> **Source:** Anurag Anshu, arXiv:2603.15495
> **Tags:** #trick #sparse-Hamiltonians #classical-optimization #uncertainty-principle #variance #energy-lowering

## What it does

Constructs a family of sparse altered Hamiltonians for classical (diagonal) Hamiltonians that achieves a **quadratic** energy-variance relationship — much stronger than the linear bound from [[Energy-Dependent Uncertainty Principle for Local Hamiltonians|the local construction]]. Useful for escaping local minima in energy landscapes with high energy barriers.

## The trick

Given a Hamiltonian $H$ diagonal in a known basis $\{|\xi_\alpha\rangle\}$ with eigenvalues $E_\alpha$ (e.g., a classical local Hamiltonian diagonal in the computational basis), define:

$$H_{T,f} = H + W^\dagger W, \quad W = \sum_{\alpha,\beta} T_{\alpha,\beta} f_{\alpha,\beta} |\xi_\alpha\rangle\langle\xi_\beta|$$

where:
- $T_{\alpha,\beta}$ is a sparsity indicator (0 or 1)
- $f_{\alpha,\beta}$ is a Gaussian with mean 0 and variance $\sqrt{E_\beta / t_\beta}$, with $t_\beta = |\{\alpha : T_{\alpha,\beta} = 1\}|$

The $W^\dagger W$ form ensures $H_{T,f}$ is PSD. By construction, $\mathbb{E}[W^\dagger W] = H$, so the altered Hamiltonians are zero-energy-preserving: $H_{T,f}|\xi_\alpha\rangle = 0$ whenever $H|\xi_\alpha\rangle = 0$.

**Two sparsity patterns for $T$:**

1. **Banded (near-diagonal):** $T_{\alpha,\beta} = 1$ iff $|\alpha - \beta| \leq t$. Creates a $2t$-banded sparse matrix. Useful for Hamiltonians with a natural ordering.

2. **Hamming-distance-1 (qubit-local):** $T_{\alpha,\beta} = 1$ iff $|\mathrm{bin}(\alpha) \oplus \mathrm{bin}(\beta)| = 1$. Each bit-flip connects a state to its single-qubit neighbours. Natural for qubit systems.

Both choices ensure $H_{T,f}$ is sparse when $H$ is a classical local Hamiltonian.

**The stronger uncertainty principle (Theorem 2.3):**

Under a continuity assumption — that neighbouring states (with $T_{\beta,\beta'} = 1$) have similar energies: $E_{\beta'} = \Omega(E_\beta) - O(t)$ — the sparse construction achieves:

$$\mathbb{E}_f\, \mathrm{Var}_\psi(H_{T,f}) = \Omega(1) \cdot \mathrm{Tr}(\psi H^2) - O(t) \cdot \mathrm{Tr}(\psi H)$$

Using $\mathrm{Tr}(\psi H^2) \geq \mathrm{Tr}(\psi H)^2$ (Jensen for PSD operators):

$$\mathbb{E}_f\, \mathrm{Var}_\psi(H_{T,f}) = \Omega(\mathrm{Tr}(\psi H)^2) - O(t) \cdot \mathrm{Tr}(\psi H)$$

This is **quadratic in energy** for $\mathrm{Tr}(\psi H) \gg t$ — qualitatively much stronger than the linear bound.

**Practical use:** For a classical Hamiltonian with oracle access to $E_\alpha$, one can simulate $H_{T,f}$ and perform energy measurements. The required Gaussian variables: if the algorithm makes $C$ queries, $C$-wise independence suffices, needing only $\mathrm{poly}(C)$ resources. For unknown ground energy $E_g$: run the algorithm multiple times, sweeping the assumed $E_g$ from 0 to maximum energy, and subtract $E_g$ from each energy measurement.

## When to reach for it

- Energy minimisation on classical (diagonal) Hamiltonians with multiple local minima
- Energy landscapes where the linear bound $\Omega(E)$ is too weak to argue progress
- Problems where strong energy barriers (multiple wells) would trap standard methods like simulated annealing
- Any scenario where $H$ is diagonal in a known basis and oracle access to $E_\alpha$ is available

The numerics in [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes|Anshu (2026)]] show this construction finding ground states of 12-qubit systems with multiple energy wells in ~9,841 energy measurements, while simulated annealing with 50,000 steps gets stuck at energy 0.46 (vs ground energy 0).

## Complexity

Constructing $H_{T,f}$: $O(n \cdot t)$ Gaussian random variables for the banded construction, or $O(n)$ for the Hamming-1 construction (each qubit has $n$ single-flip neighbours). If algorithm makes $C$ calls: $C$-wise independence uses $\mathrm{poly}(C)$ space.

Hamiltonian simulation of $H_{T,f}$: $H_{T,f}$ is sparse (sparsity $O(t)$ or $O(n)$), so standard sparse simulation applies.

## Caveat

- **Classical Hamiltonians only (in practice).** The construction is defined in the eigenbasis of $H$. For a quantum (non-diagonal) Hamiltonian, you don't know the eigenbasis, so you can't efficiently implement $H_{T,f}$. The local [[Altered Hamiltonian Family for Escaping Eigenstates]] construction is needed for quantum Hamiltonians.
- **Continuity assumption required.** Theorem 2.3 needs $E_{\beta'} = \Omega(E_\beta) - O(t)$ for neighbouring $\beta, \beta'$. This is physically reasonable for local systems but not universally guaranteed.
- **Grover consistency.** For the Grover oracle $H = I - |\alpha\rangle\langle\alpha|$, the algorithm cannot find $|\alpha\rangle$ without prior amplitude — consistent with the [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV lower bound]]. The sparse family preserves zero-energy eigenstates, so $|\alpha\rangle$ can only be reached if the initial state has overlap with it.
- No convergence guarantee for the overall alternating algorithm — the quadratic variance bound is stronger evidence but not a proof of convergence.

## Related notes
- [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes]]
- [[Energy-Dependent Uncertainty Principle for Local Hamiltonians]] — the weaker (linear) version for general local Hamiltonians
- [[Altered Hamiltonian Family for Escaping Eigenstates]] — the local analogue for quantum Hamiltonians
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — BBBV lower bound, relevant to the Grover oracle analysis
