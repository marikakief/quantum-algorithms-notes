# Review: Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferova-Mosca 2019)

Source note: [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]]

## Verdict

Minor issues. The TSAC mechanism is explained well, and the note correctly highlights the state-independent fixed operation. The remaining caveats are mostly about asymptotic versus finite-round performance and the idealized HBAC model.

## Comments

- "Optimal" should always be attached to the asymptotic HBAC cooling limit under the reset-bath model. It does not mean optimal finite-time cooling for every target qubit and hardware noise model.

- The Markov-chain framing is useful. State explicitly that the analysis concerns diagonal populations after dephasing/reset; coherences are not the resource being optimized.

- The exponential mixing time should be placed near the headline, since it is the main practical counterweight to the fixed simple compression unitary.

- The `O(n^2)` circuit for the two-sort operation is an ideal reversible-circuit cost. Real implementations depend on connectivity, reset fidelity, and repeated gate noise.

- The comparison with PPA is fair, but "PPA needs tomography" should be phrased carefully: exact partner-pairing uses state-dependent sorting of populations; implementing that adaptively in practice would require knowing or tracking those populations.

## Suggested Additions

- Add a short finite-round comparison: TSAC is robust and simple, while PPA can be greedier if one can implement the required sorting.
- Define the bath polarization convention once; different HBAC papers use `epsilon` and `tanh epsilon` differently.
