# Review: Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014)

Source note: [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]

## Primary Sources Checked

- Wiebe, Granade, Ferrie, and Cory, "Hamiltonian Learning and Certification Using Quantum Resources," Physical Review Letters 112, 190501 (2014) / arXiv:1309.0876.

## Verdict

Minor issues. The note is clear and technically useful. The main adjustment is to soften "certification" into Bayesian certification under the assumed model, prior, and likelihood.

## Line-Anchored Comments

- Lines 7-20: the computational problem is well stated. The credible region "certifies" the Hamiltonian only in the Bayesian/model-dependent sense; it is not a device-independent or worst-case certificate.
- Lines 24-30: good high-level summary. Make the trusted-simulator requirement visible in the same paragraph as the claimed quantum speedup.
- Lines 36-50: the CLE/QLE/IQLE breakdown is strong. Add that IQLE's swap or coherent interface is a serious hardware assumption, not just an implementation detail.
- Lines 52-60: the PGH rule is described well. It tunes experiments to posterior scale, but it is a heuristic, not a guarantee of globally optimal experiment design.
- Lines 62-74: the SMC update is good. Mention particle impoverishment/multimodality here, not only in caveats, because it is central to how SMC can fail.
- Lines 96-100: the Loschmidt-echo lower-bound statement should be handled carefully. PGH samples particles from the posterior, so `x^-` is close to the true `x` only after the posterior is already informative.
- Lines 108-112: the `O(d log(1/delta))` experiment scaling is empirical/numerical for the tested models, not a fully proved end-to-end complexity theorem including particle count and likelihood-estimation samples.
- Lines 124-136: the caveats are good and should remain.

## Missing or Suggested Additions

- Add an access-model box: untrusted device, trusted simulator, coherent swap/IQLE interface, known parametric Hamiltonian family, and Bayesian prior. Without that box, the word "certification" invites overinterpretation.

