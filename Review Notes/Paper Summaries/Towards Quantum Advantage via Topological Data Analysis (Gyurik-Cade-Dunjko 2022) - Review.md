# Review: Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022)

Source note: [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]]

## Primary Sources Checked

- Gyurik, Cade, and Dunjko, "Towards quantum advantage via topological data analysis," Quantum 6, 855 (2022), arXiv:2005.02607.

## Verdict

Minor-to-major issues. The note captures the DQC1-hardness backbone well, but it repeatedly lets hardness for general sparse PSD/log-local Hamiltonians sound like hardness for clique-complex Betti estimation. The caveats state the gap; the headline should too.

## Line-Anchored Comments

- Lines 11-16: good problem hierarchy. Keep the distinction between LLSD, ABNE, and exact/normalized BNE visible.
- Lines 22-28: "eliminates generic dequantization attacks" is too strong. DQC1-hardness of LLSD blocks broad dequantizations for general sparse/log-local matrices under standard assumptions, but it does not settle clique-complex ABNE.
- Lines 34-41: the histogram reduction is a clear explanation of the proof. Good.
- Lines 43-49: this is the crucial bridge to TDA. Make it explicit that it applies under the clique-density condition and uses a padded matrix construction.
- Line 53: "DQC1-hardness implies an exponential quantum speedup for rank estimation" should be softened. It is evidence/complexity-theoretic hardness under the assumption DQC1 is not classically simulable, and it depends on the input access model.
- Lines 67-71: the speedup regime should state the fixed-`k` or growing-`k` assumptions more carefully. The `tilde O(n^3)` runtime is not a generic statement for all TDA instances.
- Lines 83-89: the caveats are exactly right. Move the first caveat up into the main summary.
- Lines 93-95: the "dequantization shield" trick card should carry the same restriction: LLSD hardness is for general sparse matrices, not automatically for every combinatorial Laplacian family.

## Missing or Suggested Additions

- Add a short "what remains open" line: whether the DQC1-hardness survives under the structural restriction to clique-complex combinatorial Laplacians.

