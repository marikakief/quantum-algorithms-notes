# Review: Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015)

Source note: [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes]]

## Primary Sources Checked

- Wiebe, Granade, Ferrie, and Cory, "Quantum bootstrapping via compressed quantum Hamiltonian learning," New Journal of Physics 17, 022005 (2015) / arXiv:1409.1524.

## Verdict

Major omissions. The note is explicitly a stub and should not be treated as a finished summary.

## Line-Anchored Comments

- Lines 19-23: the key-result paragraph gives the right broad idea, but "discovers it" is too strong unless qualified. The method assumes sparsity in a known operator basis and uses compressed-sensing recovery; it does not discover arbitrary Hamiltonian structure from scratch.
- Lines 21-23: "bootstrapping" needs a formal description. The reader needs to know what the coarse estimate is, how it changes later experiment design, and what resource is saved.
- Lines 27-30: the relation to QHL and the 2012 SMC paper is useful, but the summary lacks the actual compressed-sensing measurement matrix, sparsity assumptions, and recovery guarantees.
- Lines 34-38: the TODO list correctly identifies the missing material.

## Missing or Suggested Additions

- Add the sparse Hamiltonian model, the assumed operator dictionary, sample complexity in sparsity/dimension/precision, and the compressed-sensing recovery condition.
- Add a comparison with base QHL: known low-dimensional parametric family versus sparse unknown support in a larger family.
- Add limitations: incoherence/restricted-isometry assumptions, noise sensitivity, and failure when the Hamiltonian is not sparse in the chosen basis.

