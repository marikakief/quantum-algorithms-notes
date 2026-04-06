
> **Source:** Andrew M. Childs, Yuan Su, Minh C. Tran, Nathan Wiebe, and Shuchen Zhu, *Faster quantum simulation by randomization*, Quantum **3**, 182 (2019)  
> **Links:** [Quantum](https://quantum-journal.org/papers/q-2019-09-02-182/) · [arXiv](https://arxiv.org/abs/1805.08385)  
> **Tags:** #trick #analysis #diamond-norm #randomized-algorithms

## Idea

For a randomized simulation that samples from a family of channels, the average implemented channel can look much better than any individual sample. The mixing-lemma step converts that average-channel improvement into a diamond-norm guarantee for the randomized scheme.

## The argument

Let $\{\mathcal{E}_\sigma\}_\sigma$ be the family of channels (one per permutation), and let $\bar{\mathcal{E}} = \mathbb{E}_\sigma[\mathcal{E}_\sigma]$ be the average channel. The key steps:

1. Show $\|\bar{\mathcal{E}} - \mathcal{U}_t\|_\diamond \leq \delta_{\mathrm{avg}}$ (average channel is close to target).
2. Bound $\mathbb{E}_\sigma[\|\mathcal{E}_\sigma - \mathcal{U}_t\|_\diamond] \leq \delta_{\mathrm{avg}} + \delta_{\mathrm{spread}}$ using the spread of the ensemble.
3. Conclude the randomized scheme achieves diamond-norm error $O(\delta_{\mathrm{avg}} + \delta_{\mathrm{spread}})$ over many segments.

The "mixing lemma" refers to the step that uses convexity of the diamond norm and properties of the permutation ensemble to bound $\delta_{\mathrm{spread}}$.

## When to use it

Whenever your algorithm samples from a family of circuits/channels and you've proved an average-channel improvement: this is the standard argument for converting it to a worst-case channel guarantee. The pattern recurs in [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]], randomized symmetry protection, and elsewhere.

## What to remember

The workflow is: prove average-channel improvement → bound spread/variance → invoke mixing-lemma → conclude diamond-norm guarantee. Each step is separate; conflating them leads to confusion about what's actually been proved.

## Related notes

- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]]
- [[Permutation Averaging for Product-Formula Error Cancellation]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
