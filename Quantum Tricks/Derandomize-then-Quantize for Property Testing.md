# Derandomize-then-Quantize for Property Testing

> **Source:** Ambainis, Childs, Liu, arXiv:1012.3174
> **Tags:** #trick #property-testing #derandomization #element-distinctness #quantum-walk

## What it does
Converts classical collision-based property testers into quantum algorithms with polynomial speedup, by first reducing randomness then applying quantum collision-finding.

## The trick

Many classical property testers work by:
1. Sampling random objects (e.g., random walk endpoints on a graph)
2. Looking for collisions or patterns among the samples (birthday paradox style)

The quantum conversion has two steps:

**Step 1 — Derandomize.** Replace the fully random bits with $k$-wise independent bits (for appropriate $k$). This works when the classical analysis only uses low-order moments (typically $\mathrm{E}[X]$ and $\mathrm{Var}[X]$ via Chebyshev). The key requirement: the random variable of interest depends on correlations among at most $k$ random bits. With $k$-wise independence, the number of random bits drops from $\tilde{O}(\sqrt{N})$ to $O(\text{polylog } N)$, and each sample becomes a deterministic function of an index $i$ and a seed $\omega$.

**Step 2 — Quantize.** The derandomized samples define an explicit function $f: [K] \to Y$ (indexed by sample number, computable in polylog time). Apply the [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|quantum element distinctness algorithm]] to find collisions in $\tilde{O}(K^{2/3})$ queries instead of the classical $\tilde{O}(K^{1/2})$.

For classical testers with $K = \tilde{O}(\sqrt{N})$ samples, this gives $\tilde{O}(N^{1/3})$ quantum queries.

## When to reach for it

- You have a classical property tester whose analysis relies on collision counting (or pattern detection among $O(\sqrt{N})$ samples)
- The classical analysis goes through with $k$-wise independence for moderate $k$
- You want a polynomial quantum speedup without designing a quantum algorithm from scratch

The template is: take the best classical tester → check that its moment-based analysis survives bounded independence → derandomize → plug in quantum search/element distinctness.

## Complexity

If the classical tester uses $K$ samples with evaluation cost $C$ each, the quantum version costs $\tilde{O}(K^{2/3} \cdot C)$.

For graph property testing with $K = \tilde{O}(\sqrt{N})$ and $C = \text{polylog}$: total cost $\tilde{O}(N^{1/3})$.

## Caveat

- Not all classical testers have analyses that survive $k$-wise independence. If the proof uses higher-order concentration (e.g., Chernoff bounds requiring full independence), derandomization may fail or require larger $k$.
- For counting (not just detecting) collisions, you need a wrapper that iteratively finds and removes collisions, increasing cost by a factor of the collision count $M$.
- This gives polynomial speedups only. The $\tilde{\Omega}(N^{1/4})$ lower bound for expansion testing suggests you can't do exponentially better this way for graph problems.

## Related notes
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Amplitude Estimation for Distribution Property Testing]]
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]]
