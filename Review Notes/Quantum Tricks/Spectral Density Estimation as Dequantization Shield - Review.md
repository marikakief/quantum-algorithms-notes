# Review: Spectral Density Estimation as Dequantization Shield

Source note: [[Spectral Density Estimation as Dequantization Shield]]

## Primary Sources Checked

- Gyurik, Cade, and Dunjko, arXiv:2005.02607 / Quantum 6, 855 (2022).
- Berry et al., arXiv:2209.13581 / PRX Quantum 5, 010319 (2024), for later partial dequantization context.

## Verdict

Minor issues. The idea is right, but "shield" language should be softened.

## Line-Anchored Comments

- Lines 7-16: the distinction between low-rank linear algebra and spectral-density estimation is valuable.
- Line 14: "safe from dequantization" is too strong. DQC1-hardness protects against broad generic classical simulation under standard assumptions; it does not rule out structure-specific classical algorithms.
- Lines 26-27: the caveat correctly fixes the overstatement. Move a shorter version of it into the opening section.

## Missing or Suggested Additions

- Add "general sparse PSD matrices" versus "combinatorial Laplacians of clique complexes" as a named distinction.

