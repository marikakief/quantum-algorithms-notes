# Review: Trotter Commutator-Scaling Bound

Source note: [[Trotter Commutator-Scaling Bound]]

## Primary Sources Checked

- Childs, Su, Tran, Wiebe, Zhu, "A Theory of Trotter Error," arXiv:1912.08854: https://arxiv.org/abs/1912.08854

## Verdict

Major issues. The card captures the right modern message, but it states a local one-step error bound as if it were the full simulation complexity.

## Line-Anchored Comments

- Lines 8-12: this should say whether `S(t)` is a single product-formula step or the full `r`-step simulation. For a full simulation, the global error carries the usual `t^{p+1}/r^p` scaling, with commutator norm replacing a crude `||H||^{p+1}` factor.
- Line 14: "no locality assumption needed" is correct for the representation's generality, but locality is often what makes the commutator sum small enough to be useful.
- Line 18: "geometrically" is too vague. Say "with the number of nonzero overlapping commutator patterns" or give a concrete local-lattice scaling.
- Line 28: for `p=1`, be precise: the leading commutators are ordinary pair commutators; for `p=2`, third-order nested commutators appear.

## Missing or Suggested Additions

- Add the `r`-step formula. Without it, a reader cannot convert the commutator estimate into a Trotter step count.
