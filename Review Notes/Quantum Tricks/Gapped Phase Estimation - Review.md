# Review: Gapped Phase Estimation

Source note: [[Gapped Phase Estimation]]

## Primary Sources Checked

- Kitaev phase/eigenvalue measurement source: https://arxiv.org/abs/quant-ph/9511026
- SOSSA source cited by card: https://arxiv.org/abs/2505.01528

## Verdict

Major issues. The card title sounds like generic phase estimation, but the source and content describe a newer ancilla-free polynomial/eigenphase classifier.

## Line-Anchored Comments

- Lines 2-3: tags/source

  The card is not a general "gapped phase estimation" card in the Kitaev/textbook sense. It is specifically an SOSSA-style ancilla-free interval classifier. Rename or add a first-line disambiguation.

- Lines 6 and 11: "no ancilla qubits"

  This is the distinctive claim and should be tied tightly to the SOSSA lemma. Do not let backlinks from older notes imply that Kitaev/CEMM phase estimation is ancilla-free.

- Line 9: "controlled applications of U and U^\dagger"

  If no ancilla is used, "controlled" is likely the wrong word unless the paper's construction genuinely uses controls in another register. Phrase as "a sequence of uses of `U` and `U^\dagger`/polynomial transformation" unless verified from Lemma 6.

- Lines 16 and 22: adaptive phase estimation

  Good, but chaining binary classifiers gives interval localization, not the same output distribution or guarantees as standard QPE. State the distinction.

## Missing or Suggested Additions

- Add "Not the default card for ordinary QPE" and link ordinary QPE to [[Iterative Phase Estimation (Kitaev)]] or a separate textbook-QPE card.
