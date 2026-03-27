
> **Source:** Schmidhuber, O'Donnell, Kothari, Babbush, arXiv:2406.19378
> **Tags:** #trick #guiding-state #planted-inference #amplitude-amplification #overlap #state-preparation

## What it does

Constructs a guiding state for the Guided Sparse Hamiltonian problem directly from the input of a planted inference problem, achieving quadratically better overlap with the target eigenspace than a random state.

## The trick

Given a planted $k$XOR instance $\mathcal{I}$ with $m$ constraints $(S_i, b_i)$, construct the sparse vector:

$$|\psi\rangle = \frac{1}{\sqrt{m}} \sum_{i=1}^{m} b_i |S_i\rangle \in \mathbb{C}^{\binom{n}{k}}$$

This state is correlated with the planted solution because $\langle \psi | z^{\odot k} \rangle^2 = \text{adv}_\mathcal{I}(z)^2 \cdot \frac{m}{\binom{n}{k}}$. For $m = \widetilde{\Theta}(n^{k/2})$ and constant planted advantage, this overlap is $\widetilde{\Theta}(n^{-k/2})$.

To get a guiding state in the $\binom{n}{\ell}$-dimensional Kikuchi space, take $c = \ell/k$ copies and symmetrize:

$$|\Psi\rangle \propto \sum_{T \in \binom{[n]}{\ell}} \sum_{\{S_1, \ldots, S_c\} \in \text{Part}_k(T)} \left(\prod_{j=1}^c B_\mathcal{I}(S_j)\right) |T\rangle$$

The overlap with the cutoff eigenspace is then $\widetilde{\Theta}(n^{-\ell/2})$, compared to $\Theta(n^{-\ell})$ for a random vector. This quadratic improvement in overlap translates directly to a quadratic runtime improvement via amplitude amplification, on top of the quadratic speedup from amplitude amplification itself.

**Preparation cost:** $O(\ell m \log n)$ gates, $O(\ell \log n)$ qubits. The circuit prepares $\ell/k$ copies of the $m$-sparse state $|\psi\rangle$ (using standard sparse state preparation), then projects onto the all-distinct subspace and sorts. The projection succeeds with probability $\Omega(1/\ell^{\ell/2})$, handled by the outer amplitude amplification.

## When to reach for it

- Any planted inference problem where the input data (constraint right-hand sides, tensor entries, observed labels) is correlated with the planted solution.
- When the natural "witness" for the planted case is a tensor product $z^{\otimes \ell}$ and you can approximate individual tensor factors from the input.
- More broadly, whenever you have an efficiently preparable state that's correlated with the ground state of a Hamiltonian — even polynomial (not exponential) overlap improvement is enough for super-quadratic quantum speedup.

## Complexity

State preparation: $O(\ell m \log n)$ gates. The dominant cost is repeated applications within amplitude amplification: total gate complexity $n^{\ell/4} \cdot \text{poly}(n)$.

## Caveat

The guiding state is correlated with the Kikuchi Hamiltonian (both are constructed from the same input). This correlation breaks naive composition of the overlap bounds. The solution is [[Independent Batch Splitting for Hamiltonian-Guiding State Decoupling|batch splitting]] — construct the Hamiltonian and guiding state from independent portions of the input.

The overlap bound requires $m = \Omega(n^{k/2})$ — below this threshold, the guiding state degrades to no better than random.

## Related notes
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]]
- [[Kikuchi Method for Degree Reduction]]
- [[Independent Batch Splitting for Hamiltonian-Guiding State Decoupling]]
- [[Guided Sparse Hamiltonian Framework]]
- [[Kaiser Window Amplitude Estimation]]
- [[ASCI Overlap Estimation for QPE Initial State]]
