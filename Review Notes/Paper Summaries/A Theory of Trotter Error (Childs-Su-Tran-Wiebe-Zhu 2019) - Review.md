# Review: A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019)

Source note: [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]

## Primary Sources Checked

- Childs, Su, Tran, Wiebe, Zhu, "A Theory of Trotter Error," arXiv:1912.08854: https://arxiv.org/abs/1912.08854

## Verdict

Sound with minor caveats. This is one of the better notes in the vault: the central commutator-structure message is right and the practical interpretation is mostly balanced.

## Line-Anchored Comments

- Lines 12-20: the displayed `O(alpha_comm t^{p+1})` should say whether this is a single product-formula step. For an `r`-step simulation, the global error has the usual `t^{p+1}/r^p` conversion.
- Line 24: "no locality ... assumption" is correct for the finite representation, but locality is still what makes many of the strongest applications small. Keep both statements visible.
- Lines 47-55: the practical takeaway is appropriately nuanced. This is the right counterweight to the earlier vault habit of saying qubitization simply supersedes Trotter.
- Line 78: Campbell/qDRIFT is a 2019 PRL from a 2018 arXiv preprint. The current link label `Campbell (2019)` is fine; keep date conventions consistent elsewhere.

## Missing or Suggested Additions

- Add the `r`-step global-error formula directly after the single-step commutator bound.
- Add one explicit local Hamiltonian example showing why most nested commutators vanish.
