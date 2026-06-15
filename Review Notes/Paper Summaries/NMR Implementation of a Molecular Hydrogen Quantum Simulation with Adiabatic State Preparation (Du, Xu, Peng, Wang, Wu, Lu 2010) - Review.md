# Review: NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du-Xu-Peng-Wang-Wu-Lu 2010)

Source note: [[NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) — Paper Notes]]

## Primary Sources Checked

- Du, Xu, Peng, Wang, Wu, Lu, "NMR implementation of a molecular hydrogen quantum simulation with adiabatic state preparation," PRL 104, 030502 (2010).

## Verdict

Minor issues. The note gives a clear account of the early NMR chemistry demonstration and is appropriately cautious about scalability.

## Line-Anchored Comments

- Lines 17-24: the summary is accurate, including the 45-bit phase-estimation claim. Keep the distinction between phase/energy precision within the model Hamiltonian and full chemical accuracy.
- Lines 32-39: the Hamiltonian-reduction discussion is good. Spell out that the experiment is not using a scalable second-quantized encoding in the later O'Malley sense.
- Lines 49 and 73-76: the number of adiabatic/Trotter steps and the dominant error source should be tied to the paper's reported experimental settings. These numbers are easy to overinterpret.
- Lines 65-67: "45 bits" needs context: NMR interferometry can estimate a phase with very fine precision in this tiny, highly controlled system; it is not a generic 45-bit scalable quantum chemistry computation.
- Lines 101-105: the limitations section is exactly the right tone. The NMR coherence caveat is especially important.

## Missing or Suggested Additions

- Add a one-sentence comparison to Lanyon 2010: both are early non-scalable chemistry demonstrations, but Du emphasizes adiabatic state preparation plus high-precision NMR phase estimation.
