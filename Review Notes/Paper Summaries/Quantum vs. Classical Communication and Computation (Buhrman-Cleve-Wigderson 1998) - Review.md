# Review: Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998)

Source note: [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]]

## Primary Sources Checked

- Buhrman, Cleve, and Wigderson, "Quantum vs. Classical Communication and Computation," STOC 1998, arXiv:quant-ph/9802040.

## Verdict

Minor issues. The note explains the black-box-to-communication simulation well. The main correction is notation: the DISJ and EQ' results mix bounded-error, exact, zero-error, quantum, and classical symbols in a way that could confuse readers.

## Line-Anchored Comments

- Lines 9-14: good setup. The notation `N=2^n` is helpful, but the note should keep both parameters visible because the communication costs are quoted in `log N = n` units.
- Lines 19-20: good high-level contribution statement. The "running backwards" lower-bound use is one of the paper's most reusable ideas.
- Lines 27-39: excellent explanation of simulating one oracle call by communication.
- Lines 43-48: the table mapping `L` and `F` to communication problems is useful.
- Lines 57-67: notation needs cleanup. For DISJ, distinguish the bounded-error quantum upper bound from the classical randomized lower bound; `Q_0(DISJ)` reads as zero-error quantum complexity, which is not the same comparison. For EQ', distinguish exact quantum upper bound from zero-error/exact classical lower bounds.
- Lines 69-77: the PARITY and `SIGMA_d` statements are useful but should specify query complexity rather than communication complexity where appropriate.
- Lines 85-90: good historical comparison. Keep the caveat that the exponential EQ' gap is partial and exact/zero-error.
- Lines 92 and 102: the total-function polynomial-method caveat is correct for black-box/query functions. Be careful not to overstate it as a theorem about all total communication problems.
- Lines 98-101: limitations are good. The log factor in DISJ was later addressed by more direct protocols/lower-bound refinements, so mark this as historical.
- Lines 108-112: reusable ideas are well chosen.

## Missing or Suggested Additions

- Add a notation key for `Q`, `Q_0`, `R`, `C`, exact, zero-error, and bounded-error. The current note assumes too much notation memory.

