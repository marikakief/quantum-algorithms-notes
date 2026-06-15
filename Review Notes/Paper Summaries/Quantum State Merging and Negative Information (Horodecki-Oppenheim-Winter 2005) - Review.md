# Review: Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005)

Source note: [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]]

## Verdict

Minor issues. The note gives a clean account of state merging and negative conditional entropy. The main fixes are to be more precise about resource conventions: classical communication is free in the main rate statement, entanglement cost can be negative, and one-shot versus asymptotic formulas should not be blurred.

## Line-Anchored Comments

- Lines 7-17: The problem and headline result are well stated. Add the standard convention that positive `S(A|B)` means entanglement consumption and negative `S(A|B)` means entanglement generation.

- Lines 24-36: The protocol sketch is good. The random measurement is a decoupling step, but it is not an efficient implementable prescription; mark it as an existence/random-coding proof.

- Lines 38-43: The decoupling condition is exactly the right technical centerpiece. Add a sentence that Uhlmann's theorem turns decoupling from the reference into recoverability by Bob.

- Lines 47-55: Keep the one-shot proposition visually separate from the asymptotic theorem. The one-shot bound is dimension/purity based; `S(A|B)` emerges only after typicality/AEP.

- Lines 56-66: The operational proof of strong subadditivity is a valuable comment. It should be labeled as an interpretation rather than a full proof sketch.

- Lines 68-73: The mother-protocol caveat should be reworded. The issue is not simply that classical communication is costly; the mother/FQSW protocol gives a fully quantum communication resource inequality that coherently refines state merging.

## Missing or Suggested Additions

- Add the resource inequality form of state merging.
- Include the identity `S(A|B) = -I(A\rangle B)` to connect negative information with coherent information.
