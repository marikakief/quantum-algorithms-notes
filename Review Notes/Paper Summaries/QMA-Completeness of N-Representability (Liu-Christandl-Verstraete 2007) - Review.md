# Review: QMA-Completeness of N-Representability (Liu-Christandl-Verstraete 2007)

Source note: [[QMA-Completeness of N-Representability (Liu-Christandl-Verstraete 2007) — Paper Notes]]

## Primary Sources Checked

- Liu, Christandl, and Verstraete, "Quantum computational complexity of the N-representability problem: QMA complete," Physical Review Letters 98, 110503 (2007) / quant-ph/0609125.

## Verdict

Minor issues. The note gets the main result and the chemistry significance right, but the reduction is more subtle than the current one-paragraph explanation suggests.

## Line-Anchored Comments

- Lines 7-12: the high-level implication is good. Add "worst-case" and "inverse-polynomial precision" to avoid overclaiming against practical approximate 2-RDM methods.
- Lines 16-21: the definition is readable, but for fermions the 2-RDM is antisymmetric and represents all two-particle marginals in an indistinguishable-particle setting, not just a labelled trace over particles 3 through `N` in a qubit register.
- Lines 25-28: the reduction sketch is too glib. The ground energy is a linear functional of the 2-RDM, but turning this into a decision reduction uses convex optimization/separation over the representable set; it is not literally one direct query to an `N`-representability oracle.
- Lines 32-37: the distinction between global-state approaches and RDM optimization is useful. Add that VQE avoids the representability constraint by construction but inherits nonconvex optimization and measurement problems.
- Lines 41-42: "no new trick cards" is fine, but a local "RDM consistency as quantum marginal problem" card might be useful if the vault has several marginal/consistency papers.

## Missing or Suggested Additions

- Add one paragraph connecting N-representability to the quantum marginal problem and to the convex duality between energy minimization and representability/separation.

