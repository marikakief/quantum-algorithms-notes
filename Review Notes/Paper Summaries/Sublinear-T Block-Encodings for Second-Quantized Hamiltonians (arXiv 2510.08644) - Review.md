# Review: Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644)

Source note: [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2510.08644

## Verdict

Minor issues. The note accurately captures the SELECT-SWAP/direct-sampling theme and the fixed-particle-number improvement. The main corrections are title metadata and cost-model caveats.

## Comments

- The paper title on arXiv is "Block encoding with low gate count for second-quantized Hamiltonians." The vault filename's "Sublinear-T" title is descriptive; keep the official title in metadata.

- The sublinear improvement is a T-count improvement, not necessarily a total gate, depth, routing, or wall-clock improvement. The note says this in caveats; bring it into the headline.

- The `eta`-particle subspace reduction should be stated with its parameter dependence, not just as `O(L)` to `O(sqrt L)`. For chemistry-like `L ~ n^4`, the note's `O(n^2 eta^2)` expression is the safer form.

- The SWAP-UP fermionic sign trick trades T gates for Clifford/SWAP overhead. That may be attractive in fault-tolerant accounting but is not free on constrained layouts.

- The oracle split between sparsity oracle and amplitude oracle is important. Keep their distinct architectures visible so readers do not think SELECT-SWAP solves both in the same way.

## Suggested Additions

- Add a version/date line because this is a 2025 preprint.
- Add a table separating T-count, Clifford count, ancilla count, and subnormalization.
