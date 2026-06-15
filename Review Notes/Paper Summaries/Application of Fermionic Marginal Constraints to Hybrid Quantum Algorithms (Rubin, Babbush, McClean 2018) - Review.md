# Review: Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin-Babbush-McClean 2018)

Source note: [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]

## Primary Sources Checked

- Rubin, Babbush, and McClean, "Application of fermionic marginal constraints to hybrid quantum algorithms," New Journal of Physics 20, 053020 (2018) / arXiv:1801.03524.

## Verdict

Minor issues. This is a strong and detailed note. The most important correction is about the sample-complexity formula: the exact optimal allocation involves the term variances, while the `Lambda` expression is an upper bound and not the literal minimum in general.

## Line-Anchored Comments

- Lines 7-15: the problem framing is good. Add that the naive "per term" language assumes independent Pauli/RDM-element estimation; grouped or correlated estimators change the covariance structure.
- Lines 60-74: the optimal allocation derivation is basically right, but the final bound should be stated more carefully. The optimum for independent estimators is proportional to `(sum_l |w_l| sigma_l)^2 / epsilon^2`; replacing `sigma_l <= 1` gives the `Lambda^2 / epsilon^2` bound.
- Lines 76-92: good explanation of the LP. Make clear that reducing the `L1` coefficient norm reduces a worst-case variance proxy, not necessarily the true variance of a particular ansatz state after grouping/covariances.
- Lines 94-120: the projection algorithms are accurately summarized, but "physicality restoration" should be treated as heuristic error mitigation. DQG constraints are necessary, not sufficient, and projecting noisy data can move the estimate toward a different physical-looking RDM.
- Lines 122-130: the concentration result is useful, but "random circuits are useless for chemistry" is too sweeping. The theorem is about Haar-random states or highly random ansatze, not every structured random or problem-inspired circuit.
- Lines 136-142: revise "minimum total samples satisfies" to distinguish the exact allocation formula from the `Lambda` upper bound. This is the main technical nit.
- Lines 150-160: the comparison table is helpful. Add that later basis-rotation grouping papers found RDM constraints did not necessarily reduce actual variance as dramatically as the bound suggested.
- Lines 165-173: the limitations are well chosen and should stay.

## Missing or Suggested Additions

- Add a small "bound versus actual variance" warning. This paper is historically important, but later measurement papers make clear that coefficient-norm reductions and experimental shot reductions are not identical quantities.

