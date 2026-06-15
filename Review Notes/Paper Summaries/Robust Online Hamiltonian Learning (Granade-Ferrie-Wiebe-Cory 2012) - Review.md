# Review: Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012)

Source note: [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes]]

## Primary Sources Checked

- Granade, Ferrie, Wiebe, and Cory, "Robust online Hamiltonian learning," New Journal of Physics 14, 103013 (2012) / arXiv:1207.1655.

## Verdict

Major omissions. The source note is explicitly marked as a stub, and it is not yet a usable paper summary.

## Line-Anchored Comments

- Lines 19-28: the key-result paragraph is a reasonable placeholder, but it does not describe the likelihood model, experiment design, sequential Monte Carlo update, or what "robust" means quantitatively.
- Lines 23-26: the feature bullets are too generic. "Adaptive" should name the utility or information-gain criterion, and "robust" should identify the noise/modeling assumptions.
- Lines 28-36: the relation to later QHL/RFPE notes is useful, but cross-links are not a substitute for the paper's own algorithm.
- Lines 40-44: the TODO list correctly identifies the missing core. Until those items are filled, this note should remain marked as incomplete in the index.

## Missing or Suggested Additions

- Add the SMC particle representation, Bayesian weight update, resampling rule, and experiment-design objective.
- Add the paper's convergence or robustness claims with their assumptions.
- Add a short comparison to the 2014 QHL papers: this 2012 work does online Bayesian learning from experimental data, while QHL adds quantum resources for likelihood evaluation and certification of larger quantum simulators.

