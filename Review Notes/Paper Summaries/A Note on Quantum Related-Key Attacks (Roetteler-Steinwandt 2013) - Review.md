# Review: A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013)

Source note: [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]]

## Primary Sources Checked

- Rötteler and Steinwandt, "A note on quantum related-key attacks", arXiv:1306.2301v2 (2013).

## Verdict

Minor issues. The summary is accurate and has the right emphasis: the attack is a Simon reduction under a very strong coherent related-key model. The caveats are important enough that they should appear before any "polynomial-time key recovery" sentence.

## Line-Anchored Comments

- Lines 11-29: good statement of the related-key oracle. It is worth saying "Q2 related-key access" or "coherent superposition access" in the problem statement itself.
- Lines 31-35: the assessment is correct. This is a warning about the model, not a practical AES attack.
- Lines 43-58: the function `f_s` is the right Simon oracle. The unordered-pair notation is mathematically concise, but a quantum oracle must implement a deterministic canonical encoding.
- Lines 60-70: the reversible canonical-pair implementation is a good detail. Comparison and conditional swap are polynomial, but not free in resource estimates.
- Lines 71-79: Simon sampling and linear solving are correctly described.
- Lines 85-100: the theorem statement is good. Keep the `s != 0` case separated because Simon's promise is two-to-one only for nonzero period.
- Lines 119-126: the comparison to later Q1/Q2 cryptanalysis is helpful.
- Lines 130-135: excellent caveats. Move the coherent-access caveat to the first paragraph if the note is ever abridged.

## Missing or Suggested Additions

- Add one sentence that the known plaintext-ciphertext tuple is used to enforce injectivity modulo the hidden period; without unicity the Simon promise can fail.
- The note could cross-link to the AES Grover resource estimate as a contrasting model: ordinary known-plaintext key search versus coherent related-key oracle access.

