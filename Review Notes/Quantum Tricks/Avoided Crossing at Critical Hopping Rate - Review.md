# Review: Avoided Crossing at Critical Hopping Rate

Source note: [[Avoided Crossing at Critical Hopping Rate]]

## Primary Sources Checked

- Childs and Goldstone, "Spatial search by quantum walk", arXiv:quant-ph/0306054: https://arxiv.org/abs/quant-ph/0306054

## Verdict

Major issues from overstatement. The avoided-crossing story is a good teaching picture for Childs-Goldstone search, but the card states high-dimensional approximations as if they were exact and adds statistical-mechanics language more strongly than the source supports.

## Line-Anchored Comments

- Line 7: "marked state transitions from being the ground state..."

  Convention-sensitive. Depending on whether the Hamiltonian is written as `gamma L - |w><w|` or its negative, "ground" and "first excited" reverse. Use "two relevant extremal/low-energy states under the chosen convention" unless the convention is fixed.

- Line 13: `(|w> ± |s>)/sqrt(2)` is not generally true.

  This is the high-dimensional/complete-graph intuition. In `d=4` there are logarithmic overlap losses; for `d<4` the marked-state overlap vanishes. State the dimension condition directly.

- Line 19: "same critical dimension as phi^4 field theory"

  This analogy is not needed and is easy to oversell. The paper's actual threshold comes from convergence/divergence of lattice Green's-function sums, especially `S_{2,d}`.

- Line 30: "success probability is at most O(1/N) regardless of evolution time"

  Too strong as written. Away from the tuned window the simple two-level search mechanism fails, but a global all-time upper bound of this exact form is not established by the card and depends on what "away" means.

- Line 32: good low-dimensional caveat.

  Keep this, but connect it to the `S_{2,d}` divergence rather than only to the gap.

## Missing or Suggested Additions

- Replace the phase-transition paragraph with a short derivation: `S_{1,d}` sets the critical `gamma`; `S_{2,d}` controls whether the gap/overlap gives `sqrt(N)` search.
