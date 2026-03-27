
> **Source:** Schmidhuber, O'Donnell, Kothari, Babbush, arXiv:2406.19378; Gharibian & Le Gall, STOC 2022
> **Tags:** #trick #guided-Hamiltonian #sparse-Hamiltonian #phase-estimation #amplitude-amplification #BQP-complete

## What it does

Solves the decision version of the sparse Hamiltonian problem — does $\|H\| \geq \lambda$ or $\|H\| \leq (1-\alpha)\lambda$? — given a guiding state $|\Psi\rangle$ with overlap $\gamma^2$ with the cutoff eigenspace. Total query complexity: $\widetilde{O}(s / (\gamma \alpha \lambda))$ where $s$ is the sparsity.

## The trick

Three standard subroutines composed in a specific way:

1. **Sparse [[Hamiltonian simulation]].** Given an $s$-sparse Hamiltonian $H$ on $N$ qubits with $\|H\|_{\max} \leq 1$, implement $e^{-iHt}$ to error $\epsilon$ using $O(st + \log(1/\epsilon))$ queries to the adjacency matrix and adjacency list oracles (Berry, Childs, Kothari 2015; [[Qubitization (Quantum Walk for Spectral Encoding)|Gilyén et al. 2019]]).

2. **Phase estimation.** Apply QPE to the simulated unitary $U = e^{iH\pi/(2s)}$ with the guiding state $|\Psi\rangle$ as input. The eigenphases of $U$ are in 1-1 correspondence with eigenvalues of $H$ (since $\|H\| \leq s$). Distinguish eigenphases corresponding to $h \geq \lambda$ from $h \leq (1-\alpha)\lambda$ with precision $\epsilon_{\text{PE}} = \alpha\lambda\pi/(2s)$, using $O((1/\epsilon_{\text{PE}}) \log(1/\delta_{\text{PE}}))$ applications of $U$.

3. **Amplitude amplification.** The probability of measuring a "high" eigenphase is $\geq \gamma^2$ (YES case) or 0 (NO case). Amplify with $O(1/\gamma)$ repetitions of QPE + reflection about $|\Psi\rangle$.

**Total query complexity:**

$$Q = \widetilde{O}\left(\frac{s}{\gamma \alpha \lambda}\right)$$

**Gate complexity:** $\widetilde{O}(G/\gamma + Q \cdot N)$ where $G$ is the gate cost of preparing $|\Psi\rangle$.

**Qubit count:** $A + O(N) + \widetilde{O}(\log Q)$ where $A$ is the ancilla count for $|\Psi\rangle$ preparation.

## When to reach for it

- Planted inference problems that map to sparse Hamiltonians with data-derived guiding states (Planted Noisy $k$XOR, Tensor PCA, potentially community detection, random CSP certification).
- Any setting where you have a sparse Hamiltonian and a state with $1/\text{poly}(N)$ overlap with the top eigenspace. The overlap doesn't need to be large — even $\gamma^2 = 1/N^{c}$ for constant $c$ gives polynomial-time algorithms.
- Deciding ground-state energy questions for BQP-complete problems with polynomial-overlap guiding states.

## Complexity

The framework is efficient whenever: (a) the Hamiltonian entries are efficiently computable, (b) the guiding state is efficiently preparable, and (c) the overlap is at least $1/\text{poly}(N)$. For planted $k$XOR with the [[Kikuchi Method for Degree Reduction|Kikuchi Hamiltonian]] and [[Input-Derived Guiding State for Planted Problems|input-derived guiding state]], this gives $n^{\ell/4} \cdot \text{poly}(n)$ gate complexity.

## Caveat

The framework assumes the YES/NO promise gap $\alpha$ is known in advance. In practice, the gap comes from the classical Alice Theorem and is typically $\Theta(\rho)$ (the planted advantage), which is a known parameter.

If the guiding state has overlap $\gamma^2 = 2^{-N}$ (like a random state), the problem is QMA-hard. The framework is only useful when you can prepare a state with polynomial overlap — and finding natural problems where this is possible remains an open challenge. Planted inference is currently the main known source.

## Related notes
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]]
- [[Kikuchi Method for Degree Reduction]]
- [[Input-Derived Guiding State for Planted Problems]]
- [[Independent Batch Splitting for Hamiltonian-Guiding State Decoupling]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Kaiser Window Amplitude Estimation]]
