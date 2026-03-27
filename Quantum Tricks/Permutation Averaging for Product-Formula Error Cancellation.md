
> **Source:** Andrew M. Childs, Yuan Su, Minh C. Tran, Nathan Wiebe, and Shuchen Zhu, *Faster quantum simulation by randomization*, Quantum **3**, 182 (2019)  
> **Links:** [Quantum](https://quantum-journal.org/papers/q-2019-09-02-182/) · [arXiv](https://arxiv.org/abs/1805.08385)  
> **Tags:** #trick #randomization #product-formulas #trotter

## Idea

Draw a fresh random permutation of the Hamiltonian terms at each segment of a [[Order-Condition Cancellation in Product Formulas|product formula]]. The average channel over permutations has stronger cancellation of leading error terms than any fixed ordering, giving provably better bounds in the averaged sense.

## Why it works

A deterministic ordering commits to a specific sequence-dependent bias: the same ordering-dependent commutator terms contribute the same signed error on every segment. Randomizing the permutation turns those bias terms into a sum over signed contributions from different orderings. Many of these cancel by symmetry — specifically, the nondegenerate Taylor terms (those involving all-distinct summand indices) average to zero over the symmetric group.

What survives averaging: "degenerate" terms with repeated summand indices, which are permutation-invariant. These have a smaller combinatorial coefficient than the sum of all terms, giving the improved bound.

## When useful

- Hamiltonian is a sum of many terms and [[Order-Condition Cancellation in Product Formulas|product formula]] are otherwise attractive.
- The main bottleneck is sensitivity to term ordering.
- Moderate precision; not the high-precision regime where [[Order-Condition Cancellation in Product Formulas|product formula]] are already losing.

## What it does not do

Does not make [[Order-Condition Cancellation in Product Formulas|product formula]] universally competitive — it's an improvement within the product-formula regime, not an escape from it. High-order formulas or adversarial Hamiltonians still need heavier machinery.

## Related notes

- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes]]
- [[Nondegenerate-vs-Degenerate Taylor-Term Bookkeeping]]
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]]
- [[Monte-Carlo Permutation Error Estimation for Product Formulas]]
