# Review: Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019)

Source note: [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]

## Primary Sources Checked

- Berry, Gidney, Motta, McClean, Babbush, "Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization", arXiv:1902.02134 / Quantum 3, 208 (2019).
- Compared against Babbush et al. 2018 PRX and Lee et al. 2021 THC notes.

## Verdict

Major issues. The core narrative is sound, but the asymptotic statements omit precision/normalization in a few places and the comparison table includes a misleading FeMoCo entry for the plane-wave-dual algorithm.

## Line-Anchored Comments

- Line 23: `~O(N^{3/2} lambda)` is shorthand for phase-estimation Toffoli complexity and should include the energy precision denominator, e.g. `~O(N^{3/2} lambda / Delta E)` in the many-ancilla single-factorization regime.
- Line 27: the "700x" improvement is a surface-code spacetime comparison, not simply a Toffoli-count comparison. Keep the wording tied to spacetime volume.
- Lines 37-45: the single-factorization description is good. The note correctly emphasizes chemist ordering and positive semidefiniteness.
- Line 41: `L = O(N)` is empirical/physical, not a theorem for arbitrary molecular bases. This caveat appears later at line 143 but should also be next to the first use.
- Lines 67-71: the QROAM cost statements are among the most important technical contributions. They should specify Toffoli counts, since the predecessor paper used T-gate language.
- Lines 101-109: the concrete table is useful, but avoid comparing raw Toffolis to Reiher et al. as if all resource-estimation stacks are identical. The paper itself frames this carefully through physical resource assumptions.
- Lines 111-114: the leading-order complexities need more notation. `M` and `L` are not explained in the key-results section, and both expressions should include `/ Delta E`.
- Line 125: the Babbush-Gidney PRX row with `~10^8` "FeMoCo Toffolis" is misleading. That algorithm is for the plane-wave-dual electronic-structure Hamiltonian, not an arbitrary molecular-orbital FeMoCo active-space Hamiltonian. It should not be placed in the FeMoCo column without a strong caveat.
- Line 147: "second factorization not exploited" is right for this paper, but later arbitrary-basis qubitization did exploit related structure through double factorization and THC. Mention von Burg et al. 2020/2021 as well as Lee et al. 2021 if this caveat is intended as a historical bridge.

## Missing or Suggested Additions

- Add a small glossary for `L`, `M`, `lambda`, `Delta E`, `L_V^(c)`, and "dirty ancillae"; the note is otherwise too easy to misread.
- In the sparse variant, emphasize that thresholding is an empirical approximation validated by classical energy proxies, not a rigorous worst-case sparsity theorem.

