# Review: Experimental Quantum Hamiltonian Learning (Wang-Paesani-Santagati et al. 2017)

Source note: [[Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) — Paper Notes]]

## Primary Sources Checked

- Wang et al., "Experimental Quantum Hamiltonian Learning," Nature Physics 13, 551-555 (2017) / arXiv:1703.05402.

## Verdict

Minor issues. The note is detailed and appropriately treats the experiment as validation rather than quantum advantage. A few comparative statements need tighter scoping because QLE and IQLE were demonstrated in different experimental configurations.

## Line-Anchored Comments

- Lines 7-11: the computational problem is clear. Add that the concrete learned model is single-qubit/one- or two-parameter, so the hard likelihood-evaluation regime is not reached.
- Lines 17-25: strong summary. The model-deficiency discussion is good, but posterior variance saturation can also indicate noise, drift, or likelihood misspecification; chirp is the model selected in this experiment, not a universal diagnostic outcome.
- Lines 23-25: the "two orders of magnitude better than QLE" IQLE comparison should be scoped. The QLE demonstration learns an NV center through a photonic simulator, while IQLE self-verification is on the photonic chip itself.
- Lines 31-37: useful hardware details. The 3 million repetitions per NV measurement should be brought forward because it frames the practical cost of the proof of concept.
- Lines 41-54: the entanglement-based overlap-estimation description is good. The note correctly flags that this is a forward-only variant rather than a literal implementation of the original time-reversal IQLE circuit.
- Lines 56-60: SMC with 20 particles is fine for this toy parameter space; make clear it tells us little about scaling to high-dimensional Hamiltonians.
- Lines 62-76: the results table is helpful. Consider adding a "classically trivial?" column or sentence to avoid implying computational advantage.
- Lines 94-106: the limitations are excellent and should remain.

## Missing or Suggested Additions

- Add a compact access-model comparison: QLE with separate NV and photonic simulator through classical communication; IQLE self-verification on one photonic chip; original theory requiring coherent swap/interface.

