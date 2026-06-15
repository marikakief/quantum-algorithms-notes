# Review: Recursive Group-Commutator Product Formula

Source note: [[Recursive Group-Commutator Product Formula]]

## Primary Sources Checked

- Childs and Wiebe, arXiv:1211.4945: https://arxiv.org/abs/1211.4945

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 7-14: the base group-commutator formula is correct.
- Lines 17-28: the odd-`k` recursion and special `k=1` symmetrization are explained well.
- Line 34: the user may have access to `e^{Bt}` rather than `e^{Bt^k}` directly; the construction rescales the available `B` evolution time. Make that explicit.
- Line 48: the comparison to LCU/QSP should be scoped. This paper solves commutator-exponential simulation from elementary exponentials, not ordinary Hamiltonian simulation from a block-encoding.

## Missing or Suggested Additions

- Add one concrete example of why commutator exponentials are useful, such as generating next-nearest-neighbor hopping from nearest-neighbor terms.
