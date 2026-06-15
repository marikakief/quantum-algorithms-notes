# Review: qDRIFT Randomized Hamiltonian Simulation (Campbell 2018)

Source note: [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]

## Primary Sources Checked

- Campbell, "A random compiler for fast Hamiltonian simulation", PRL 2019 / arXiv:1811.08017.

## Verdict

Minor issues. The note is accurate and concise. It correctly emphasizes that qDRIFT tracks the coefficient 1-norm and outputs an average channel rather than a deterministic circuit.

## Line-Anchored Comments

- Add a top-level title; the file starts with a blank line and metadata.
- Lines 10-14: good summary of the `lambda` cost driver. "Common in second-quantized chemistry" should be softened to "can occur"; qDRIFT's advantage is Hamiltonian-instance dependent.
- Lines 18-27: protocol and `N=O((lambda t)^2/epsilon)` scaling are correct at this level. Include the constant-error bound if this note will be used for resource estimates.
- Lines 31-33: good relationship to earlier randomized simulation work.
- Lines 35-43: comparison with deterministic product formulas is fair. Mention that deterministic formulas can win when commutators/locality are favorable.
- Lines 47-50: caveats are all important. The "expected channel, not individual sample paths" point should be near the protocol too.
- Lines 76-78: later cross-links are useful but should not be mixed with in-paper references if the section title is interpreted literally.

## Missing or Suggested Additions

- Add the diamond-norm/channel proof idea: the average over sampled one-term evolutions matches the first-order generator, and channel composition gives the stated error.
