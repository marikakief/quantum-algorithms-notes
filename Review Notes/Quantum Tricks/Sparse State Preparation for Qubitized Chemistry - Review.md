# Review: Sparse State Preparation for Qubitized Chemistry

Source note: [[Sparse State Preparation for Qubitized Chemistry]]

## Primary Sources Checked

- Berry et al., arXiv:1902.02134, sparse Coulomb construction.

## Verdict

Minor issues. The sparse-QROAM idea is well summarized. The main risk is overgeneralizing an empirical thresholding technique into a robust asymptotic method.

## Line-Anchored Comments

- Lines 5-9: good motivation. The note correctly distinguishes nonzero-entry count from the full tensor dimension.
- Lines 13-17: the modified alias-sampling procedure is plausible. Add that the QROAM table must output the physical tuple as well as alias/keep data, increasing output width.
- Lines 19-24: the eightfold-symmetry controlled-swap explanation is useful. It should mention signs/order conventions if this is later applied outside real chemist-ordered Coulomb tensors.
- Line 30: "without introducing chemical-accuracy errors" is too strong. The paper chooses thresholds by monitoring classical proxies such as CISD/MP2 energies; this is empirical validation, not a rigorous guarantee for every target eigenvalue.
- Line 35: "Works with any LCU-based simulation" is overbroad. The sparse alias idea is general, but the symmetry and truncation benefits are chemistry-structure dependent.
- Lines 37-40: good complexity lines. Specify clean-ancilla QROAM when quoting `O(sqrt(d M))`.
- Line 43: the caveat is exactly right: sparsity is not a worst-case asymptotic improvement.

## Missing or Suggested Additions

- Mention that thresholding changes the Hamiltonian and therefore contributes a separate Hamiltonian-approximation error budget, distinct from QPE/block-encoding error.

