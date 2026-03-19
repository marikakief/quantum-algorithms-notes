
> **Source:** Andrew M. Childs, Yuan Su, Minh C. Tran, Nathan Wiebe, and Shuchen Zhu, *Faster quantum simulation by randomization*, Quantum **3**, 182 (2019)  
> **Links:** [Quantum](https://quantum-journal.org/papers/q-2019-09-02-182/) · [arXiv](https://arxiv.org/abs/1805.08385)  
> **Tags:** #hamiltonian-simulation #product-formulas #randomization #trotter

---

## What the paper does

Shows that randomizing the ordering of Hamiltonian terms in a product formula gives provably stronger error bounds than any fixed ordering, sometimes asymptotically better than previous commutator-based bounds despite using less information about the Hamiltonian structure.

The key claim from the abstract: bounds from the randomized approach "can be asymptotically better than previous bounds that exploit commutation between the summands, despite using much less information about the structure of the Hamiltonian."

## Main idea

For $H = \sum_{j=1}^L H_j$, a deterministic product formula fixes one permutation. This paper instead draws a fresh random permutation $\sigma$ for each segment and analyzes the average channel over these random circuits.

The improvement comes from cancellation: terms in the Taylor expansion of the error that depend on the ordering choice cancel when averaged over permutations. After averaging, the dominant error terms are those that survive independently of ordering — a smaller set.

The analysis proceeds in three steps:
1. Expand the product formula error in a Taylor series.
2. Average over permutations and identify which terms cancel.
3. Use a mixing-lemma argument to convert average-channel improvement into a worst-case diamond-norm guarantee.

The distinction between nondegenerate terms (all summand indices distinct, hence permutation-sensitive) and degenerate terms (repeated indices, permutation-invariant) drives the cancellation bookkeeping.

## What this does and doesn't do

Still a product-formula method — no block-encoding, no qubitization. Still easy to compile. The improvement is in how the error scales with $L$ and the ordering-sensitive commutator structure.

This is not the same as qDRIFT, which samples single terms by weight. Here the basic unit is a full shuffled product formula, not one random exponential.

| Method | What randomness does | Basic unit |
|---|---|---|
| qDRIFT | Samples single term by weight | One random exponential |
| This paper | Randomizes the term ordering | A full shuffled product formula |

Both are randomized simulation methods but attack different weaknesses.

## When this is useful

Most relevant when:
- product formulas are otherwise attractive (easy compilation, no appetite for block-encoding machinery),
- the main bottleneck is sensitivity to ordering in large-$L$ decompositions,
- moderate precision suffices.

For high precision or adversarial structure, product formulas are still not the right hammer regardless of randomization.

## Predecessors

Random-order heuristics for product formulas appeared earlier in numerical ODE literature (see Süli, Iserles and related work on splitting methods). This paper provides rigorous quantum information guarantees.

## References within this paper

- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2019)]] — qDRIFT; this paper extends randomization to higher-order Suzuki formulas
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2021)]] — deterministic Trotter error theory
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — Suzuki integrators
- Hastings (2018) — mixing lemma and related randomization techniques for simulation

---

## Cross-links

- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — earlier randomised product formula for time-dependent Hamiltonians via Monte Carlo time-sampling
- [[Permutation Averaging for Product-Formula Error Cancellation]]
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]]
- [[Nondegenerate-vs-Degenerate Taylor-Term Bookkeeping]]
- [[Monte-Carlo Permutation Error Estimation for Product Formulas]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]] — uses Richardson extrapolation (incoherent multiproduct) over qDRIFT step sizes; closely related to the coherent multiproduct approach here
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — combines multi-product formulas with randomized sampling; complementary approach to the permutation-averaging technique here
