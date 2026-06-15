# Review: 3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003)

Source note: [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]]

## Primary Sources Checked

- Kempe and Regev, "3-local Hamiltonian is QMA-complete," Quantum Information and Computation 3(3), 258-264 (2003) / quant-ph/0302079.

## Verdict

Major omissions. The note is directionally correct, but it is too skeletal for a foundational locality-reduction paper.

## Line-Anchored Comments

- Lines 7-10: the main result is right. Add that this is a locality reduction from Kitaev's 5-local construction while preserving an inverse-polynomial promise gap.
- Lines 14-18: the penalty idea is correct, but the note needs the actual perturbative structure: what subspace is protected, what terms are treated as perturbations, and how the low-energy spectrum approximates the original Hamiltonian.
- Lines 16-18: the bound `O(K^2/(J-2K))` is useful, but define `K` and the required scale of `J` relative to the original promise gap.
- Lines 22-24: the reusable idea is good. It should distinguish this projection/penalty lemma from the later mediator-qubit perturbation gadgets used in the 2-local result.
- Lines 28-38: cross-links are good, but the note is currently more of an index card than a paper summary.

## Missing or Suggested Additions

- Add a proof sketch with the protected legal-clock subspace, the effective Hamiltonian inside it, and the perturbation lemma's role in preserving YES/NO instances.
- Add a comparison to KKR 2006: Kempe-Regev reduces the history-state construction to 3-local by penalties; KKR uses more elaborate mediator gadgets to reach 2-local.

