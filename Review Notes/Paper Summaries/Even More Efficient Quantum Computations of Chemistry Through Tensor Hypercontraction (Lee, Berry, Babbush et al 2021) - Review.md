# Review: Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021)

Source note: [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]

## Primary Sources Checked

- Lee et al., "Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction", arXiv:2011.03494 / PRX Quantum 2, 030305 (2021).
- Checked later update context against Low et al., "Fast quantum simulation of electronic structure by spectral amplification", arXiv:2502.15882 / PRX 15, 041016 (2025).

## Verdict

Major issues. The note is good at motivation and historical placement, but the per-step scaling explanation contains an internal contradiction, and the lambda-ordering claim is too strong.

## Line-Anchored Comments

- Lines 19-21: the headline results are broadly right and well contextualized.
- Lines 37-63: the non-orthogonal THC explanation is mostly clear. However, saying "the Coulomb operator becomes diagonal" should be handled carefully: the basis is non-orthogonal, and the implementable block encoding works by term-dependent rotations, not by globally changing to a standard orthonormal second-quantized basis.
- Lines 71-75: there is a serious scaling inconsistency. The note writes a per-step cost with a leading `2M ceil((N/2)/2)` term, calls it `O(MN)=O(N^2)`, and then says the total is `~O(N lambda_zeta / epsilon)` after QROM reduction. The theorem-level claim is `~O(N)` per walk step under the THC information-content/QROM construction. Rewrite this section from the source's final cost expression rather than mixing an apparent Givens-count term with the asymptotic QROAM accounting.
- Line 86: `lambda_zeta <= lambda_DF <= lambda_V <= lambda_SF` "in general" is too strong. These norms depend on factorization choices, truncation, and optimization. The paper reports favorable empirical/order comparisons and specific inequalities in its setup; do not present the entire chain as a universal theorem without proof.
- Lines 88-100: the FeMoCo table is useful. Keep the caveat that active-space choices and later corrections affect raw comparisons.
- Lines 119-123: saying `lambda_zeta` is always the smallest in the comparison table repeats the overclaim from line 86. Phrase as "reported smallest in the benchmarked instances" unless a theorem is cited.
- Lines 218-224: the prose assessment is strong but should not say "optimal up to logarithmic factors" unless the lower-bound assumptions are stated. It is a plausible algorithmic frontier statement, not a theorem in the note.
- Line 234: the 2025 update is broadly real: the later PRX paper is titled "spectral amplification" and reports 4x to 195x speedups over state of the art. The note's "spectrum amplification" wording should be updated to the journal title if using the published citation, and any exact FeMoCo-76 Toffoli number should be checked against the final version before being used as a settled comparison.

## Missing or Suggested Additions

- Add a small warning that THC rank and factorization quality are empirical and optimizer-dependent. This is mentioned later, but it belongs next to the first `M = O(N)` claim.
- Separate "algorithmic block-encoding cost" from "compiled Givens-rotation synthesis cost" more explicitly. That is where the current scaling confusion enters.

