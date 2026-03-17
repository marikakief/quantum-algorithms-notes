
> **Tags:** #trick #analysis #diamond-norm #randomization
> **Source:** arXiv:1811.08017

## What it does

Converts an average-channel approximation (easy to prove for [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|randomized compiler]]) into a rigorous worst-case diamond-norm guarantee (what you actually need for simulation error bounds).

## The argument

For a [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|randomized compiler]] that at each step samples a channel $\mathcal{E}_j$ with probability $p_j$:
1. Show the expected channel $\bar{\mathcal{E}} = \sum_j p_j \mathcal{E}_j$ is close to the ideal target channel per step in some norm.
2. Compose $N$ steps: [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm|diamond norm]] satisfies $\|\mathcal{E}_1 \circ \cdots \circ \mathcal{E}_N - \mathcal{F}_1 \circ \cdots \circ \mathcal{F}_N\|_\diamond \leq \sum_k \|\mathcal{E}_k - \mathcal{F}_k\|_\diamond$ by subadditivity.
3. Combine per-step average error with step count $N$ to get total diamond-norm error.

The key step is that the [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm|diamond norm]] of a mixture of channels satisfies $\|\sum_j p_j \mathcal{E}_j - \mathcal{I}\|_\diamond \leq \sum_j p_j \|\mathcal{E}_j - \mathcal{I}\|_\diamond$, so linearity of expectation transfers directly.

## When to reach for it

Any randomized simulation or compilation protocol needing a strong worst-case guarantee. The approach is standard but worth stating explicitly because it links the "average channel looks good" proof to the "the actual randomized scheme is guaranteed to work" conclusion.

## Caveat

The bound can be conservative: the actual random circuit error may concentrate well below the diamond-norm bound, especially when individual sample errors partially cancel. Empirical error is often significantly better than the proven bound.

## Related Paper Notes
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]]
