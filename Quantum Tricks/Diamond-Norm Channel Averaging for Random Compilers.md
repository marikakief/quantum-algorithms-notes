
> **Tags:** #trick #analysis #diamond-norm #randomization
> **Source:** arXiv:1811.08017

## What it does

Converts an averaged-channel approximation (easy to prove for [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|randomized compiler]]) into a rigorous diamond-norm guarantee for the ensemble channel. This is the right guarantee for randomized simulation protocols, but it is not a statement that every sampled circuit is close to the target unitary.

## qDRIFT averaged-channel telescoping

For a [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|qDRIFT]] step, let $\mathcal{E}_j$ be the channel for the sampled term and $p_j$ its sampling probability. Define the one-step averaged channel
$$
\bar{\mathcal{E}}=\sum_j p_j \mathcal{E}_j
$$
and the ideal short-time channel $\mathcal{F}$ for $e^{-iH t/N}$.

The full randomized protocol is the mixture over all sampled sequences, i.e. the channel $\bar{\mathcal{E}}^N$. Once the one-step estimate $\|\bar{\mathcal{E}}-\mathcal{F}\|_\diamond$ is known, a telescoping bound gives
$$
\|\bar{\mathcal{E}}^N-\mathcal{F}^N\|_\diamond
\leq
N\|\bar{\mathcal{E}}-\mathcal{F}\|_\diamond.
$$

For qDRIFT, the Taylor expansion of $\bar{\mathcal{E}}$ matches the ideal first-order generator and leaves a second-order one-step error, giving the total $O((\lambda t)^2/N)$ bound.

## When to reach for it

Any randomized simulation or compilation protocol needing a strong worst-case guarantee for the averaged channel. The approach is standard but worth stating explicitly because it links the "average channel looks good" proof to the diamond-norm error of the randomized ensemble.

## Caveat

The bound can be conservative: sampled circuit errors may concentrate well below the ensemble diamond-norm bound, especially when individual sample errors partially cancel. But the theorem itself is an ensemble-channel guarantee.

## Stronger version: quadratic error suppression

The qDRIFT telescoping argument is separate from a stronger lemma used in later randomized compilers. The Hastings-Campbell mixing lemma gives *quadratic* suppression when random unitaries individually approximate a target unitary and their average approximates it even better.

Let $U_j$ be unitaries sampled with probabilities $p_j$, let $V$ be the target unitary, define
$$
\Lambda(\rho)=\sum_j p_j U_j\rho U_j^\dagger,\qquad
\mathcal{V}(\rho)=V\rho V^\dagger.
$$
If $\|U_j-V\|\le a$ for every $j$ and $\|\sum_j p_j U_j - V\|\le b$, then
$$
\|\Lambda - \mathcal{V}\|_\diamond \le a^2 + 2b.
$$

This means individual circuits only need error $O(\sqrt{\epsilon})$ to achieve channel error $O(\epsilon)$ — a quadratic improvement. This stronger form is what enables [[Stochastic QSP via Chebyshev Tail Truncation|Stochastic QSP]] to halve query complexity: the ensemble polynomials have pointwise error $O(\sqrt{\epsilon})$, but the channel error is $O(\epsilon)$.

See [[Mixing Lemma for Block-Encodings]] for the extension to block-encoded operators (needed for QSP/QSVT).

## Related Paper Notes
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]]
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]]
- [[Mixing Lemma for Block-Encodings]]
- [[Stochastic QSP via Chebyshev Tail Truncation]]
