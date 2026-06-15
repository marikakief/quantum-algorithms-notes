# Review: 2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006)

Source note: [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]

## Primary Sources Checked

- Kempe, Kitaev, and Regev, "The Complexity of the Local Hamiltonian Problem," SIAM Journal on Computing 35(5), 1070-1097 (2006) / quant-ph/0406180.

## Verdict

Minor omissions. The note captures the result and its importance, but it understates how much technical work is hidden in "perturbation gadget."

## Line-Anchored Comments

- Lines 7-12: good headline. The classical analogy should be phrased carefully: local Hamiltonian is an optimization/promise-energy problem, so the closer classical analogue is MAX-2-SAT-style constraint optimization rather than decision 2-SAT.
- Lines 16-22: the consequence for VQE is correct in worst-case complexity terms. Add "worst-case" explicitly; practical chemistry instances can be much easier.
- Lines 26-31: the perturbation-gadget summary is useful but too compressed. The paper's contribution is not just "add mediator qubits" but controlling the low-energy effective Hamiltonian order by order while preserving the promise gap.
- Lines 35-40: good reusable-card choices. The subdivision gadget should be described with its order of perturbation and scale separation if the trick card depends on this note.
- Lines 46-54: cross-links are good. Add Cubitt-Montanaro's classification as a later refinement of interaction-type restrictions, not a replacement for the KKR locality theorem.

## Missing or Suggested Additions

- Add a short list of assumptions in the gadget theorem: large penalty scale, mediator ground subspace, effective interaction order, and inverse-polynomial control of the residual.

