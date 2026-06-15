# Review: Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang-Dolde-Biamonte-Babbush-Bergholm et al. 2015)

Source note: [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes]]

## Primary Sources Checked

- Wang et al., "Quantum Simulation of Helium Hydride Cation in a Solid-State Spin Register," ACS Nano 9, 7769-7774 (2015) / arXiv:1405.2696.

## Verdict

Minor issues. The note accurately describes the NV-center demonstration and correctly warns that the configuration-basis encoding is not scalable.

## Line-Anchored Comments

- Lines 17-24: the platform summary is good. "Viable platform" should be read as proof-of-concept viability, not a demonstrated scalable architecture.
- Lines 22 and 68-76: the `10^-14` Hartree uncertainty is indeed in the paper's abstract, but keep the phrase "given our model basis" attached to it. Otherwise the number looks like real chemical accuracy beyond basis/model error.
- Lines 30-39: the "bond angle parameter" wording is odd for a diatomic molecule. Use bond length or the paper's exact parameterization.
- Lines 40-44: excellent caveat about configuration-basis encoding. This should be near the first paragraph because it is the main algorithmic limitation.
- Lines 73-78: the explanation of why the precision is so high is plausible, but distinguish experimental phase precision from scalable algorithmic precision.
- Lines 96-103: the limitations are strong and should stay.

## Missing or Suggested Additions

- Add a direct contrast with O'Malley et al.: Wang has extraordinary precision in a non-scalable configuration basis; O'Malley has lower precision but a scalable fermionic encoding.
