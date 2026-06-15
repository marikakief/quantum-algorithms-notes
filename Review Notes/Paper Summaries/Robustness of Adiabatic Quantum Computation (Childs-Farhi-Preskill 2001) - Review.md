# Review: Robustness of Adiabatic Quantum Computation (Childs-Farhi-Preskill 2001)

Source note: [[Robustness of Adiabatic Quantum Computation (Childs-Farhi-Preskill 2001) — Paper Notes]]

## Primary Sources Checked

- Childs, Farhi, and Preskill, "Robustness of adiabatic quantum computation," Physical Review A 65, 012322 (2002) / quant-ph/0108048.

## Verdict

Major issues in the scaling language. The qualitative picture is right, but the note overstates "gap as noise filter" and contains a confusing adiabatic-error expression.

## Line-Anchored Comments

- Lines 15-24: the two failure modes and thermalization window are good. Keep the warning that running too slowly can be bad in an open system.
- Lines 38-50: the master-equation setup is fine, but be cautious with "Lindblad / Redfield" as interchangeable. Redfield generators need not always be completely positive in the same way a Lindblad-form generator is.
- Lines 60-64: the displayed transition estimate is too schematic. Bath-induced transition rates depend on the bath spectral density at the transition frequency and instantaneous matrix elements; they are not universally suppressed as a simple `1/gamma^2` law.
- Lines 68-74: the adiabatic-error expression is wrong/confusing. For slow time `s=t/T`, unitary adiabatic error scales like a derivative/gap factor divided by `T`, not `||dot H||/gamma^2 T * T`.
- Lines 82-90: "robustness threshold" and "gap is the primary protector" should be softened. This is perturbative open-system stability, not a threshold theorem, and the gap helps only relative to the bath temperature/spectral density/coupling operators.
- Lines 108-115: the caveats are strong and should be moved closer to the headline claims.
- Lines 121-127: the reusable trick cards currently overstate universality. "Any system with an energy gap enjoys quadratic suppression" is not generally true without assumptions on the bath and coupling matrix elements.

## Missing or Suggested Additions

- Add the actual assumptions in a boxed list: weak coupling, Born-Markov approximation, bath temperature, spectral density, single or specified coupling operators, and no exponentially small gap.

