# Review: Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985)

Source note: [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]]

## Primary Sources Checked

- Deutsch, "Quantum theory, the Church-Turing principle and the universal quantum computer", Proc. R. Soc. Lond. A 400, 97-117 (1985), DOI: https://doi.org/10.1098/rspa.1985.0070
- Full-text mirror checked: https://docslib.org/doc/7169585/quantum-theory-the-church-turing-principle-and-the-universal-quantum-computer
- Bernstein-Vazirani full version for later efficient-QTM comparison: https://web.cs.miami.edu/home/burt/learning/csc595.251/Bernstein-Vazirani-QTM.pdf

## Verdict

Minor issues. The main structure is right, but the note occasionally translates Deutsch's physical Church-Turing principle into modern efficiency language too aggressively.

## Line-Anchored Comments

- Lines 13 and 26-28: "classical Turing machines violate this principle (they can't efficiently simulate quantum systems)"

  Deutsch's 1985 principle is phrased as perfect simulation of finitely realizable physical systems by finite means. The text does argue classical universal Turing machines are not compatible with the strong physical principle, but the note should not recast the whole point as an efficiency claim. Bernstein-Vazirani later supplies the complexity-theoretic efficient-QTM framing.

- Lines 54-88: Deutsch's algorithm

  The note handles the modern deterministic circuit responsibly by warning at line 88 that it is not Deutsch's exact original presentation. Keep this caveat near the start of the algorithm section so readers do not attribute the clean phase-kickback circuit to 1985.

- Line 104: "This is the trick behind ... Shor"

  Phase kickback is foundational, but Shor's algorithm is better described as Fourier sampling/order finding using reversible modular exponentiation. If keeping Shor in this sentence, phrase it as "controlled-unitary/eigenphase variants of the same idea appear in phase estimation and period finding" rather than implying Shor is just a phase-oracle algorithm.

- Lines 110-112: "Doesn't formally prove the QTM can simulate all quantum systems efficiently"

  Good caveat. Add that Bernstein-Vazirani later constructed an efficient universal QTM in Deutsch's model, while efficient simulation of arbitrary physical quantum systems still depends on locality/access assumptions.

## Missing or Suggested Additions

- Add a short "1985 vs modern circuit" box: original Deutsch algorithm, Deutsch-Jozsa exact version, and Cleve-Ekert-Macchiavello-Mosca phase-kickback presentation.
