# Review: Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020)

Source note: [[Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes]]

## Primary Sources Checked

- Chakrabarti, Childs, Li, and Xiaodi Wu, "Quantum algorithms and lower bounds for convex optimization", Quantum 4, 221 (2020) / arXiv:1809.01731.

## Verdict

Major issues. This note appears to summarize a different imagined convex-optimization paper. The actual paper's headline is an `\tilde O(n)` quantum algorithm using objective-evaluation and membership oracles for an `n`-dimensional convex body, with lower bounds around `\tilde\Omega(\sqrt n)` evaluation queries and `\Omega(\sqrt n)` membership queries. The source note instead describes subgradient methods, cutting planes, QLSA/IPM Newton steps, and Gibbs sampling over a simplex.

## Line-Anchored Comments

- Line 1: the fourth author is Xiaodi Wu, not Chunhao Wu.
- Lines 11-22: the problem model is wrong for this paper. The paper is about evaluation and membership oracles for general convex-body optimization, not gradient/subgradient oracles as the primary model.
- Lines 28-39: the listed contributions do not match the paper. Replace the whole section with the `\tilde O(n)` query algorithm and the evaluation/membership lower bounds.
- Lines 45-61: the quantum subgradient-method story should be removed unless it is explicitly moved to a different paper. Jordan gradient estimation and amplitude-estimated gradient averaging are not the central algorithm here.
- Lines 66-73: the QLSA/interior-point/cutting-plane description is not this paper's result. It reads more like a conflation with quantum SDP/IPM literature.
- Lines 75-83: the Gibbs-sampling simplex algorithm is not part of the cited paper's main result.
- Lines 86-100: all theorem labels and bounds should be replaced. The actual abstract-level theorem is `\tilde O(n)` queries for convex optimization over an `n`-dimensional convex body, a quadratic improvement over the best-known classical algorithm, plus lower bounds of `\tilde\Omega(\sqrt n)` evaluation and `\Omega(\sqrt n)` membership queries.
- Lines 123-131: the QLSA/readout/coherence caveats are aimed at the wrong algorithmic framework.
- Lines 137-178: the trick cards and cross-links should be removed or moved to a different note. They currently reinforce the incorrect summary.

## Missing or Suggested Additions

- Rewrite from scratch around the membership/evaluation oracle model.
- Include the independently obtained van Apeldoorn-Gilyen-Gribling-de Wolf result mentioned in the paper metadata.
- Add a careful "query complexity only" caveat: the paper's speedup is in oracle queries, and arithmetic/geometric implementation costs must be treated separately.

