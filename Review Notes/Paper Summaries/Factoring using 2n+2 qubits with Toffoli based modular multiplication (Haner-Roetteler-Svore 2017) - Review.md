# Review: Factoring using 2n+2 qubits with Toffoli based modular multiplication (Haner-Roetteler-Svore 2017)

Source note: [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]]

## Primary Sources Checked

- Haner, Roetteler, and Svore, "Factoring using 2n+2 qubits with Toffoli based modular multiplication", arXiv: https://arxiv.org/abs/1611.07995
- ar5iv rendering of the paper: https://ar5iv.labs.arxiv.org/html/1611.07995

## Verdict

Major issues, narrowly. The note's account of the 2017 paper is mostly accurate, but it contains a materially wrong comparison to later Gidney-Ekera resource counts and a few source-scope overstatements.

## Line-Anchored Comments

- Lines 17-19: headline contribution

  Correct. The abstract states `2n+2` qubits, Toffoli-based modular multiplication, depth `O(n^3)`, overall gate count `O(n^3 log n)`, and a dirty-ancilla in-place constant adder of size `O(n log n)` and depth `O(n)`.

- Lines 38-44: dirty-ancilla incrementer and recurrence

  The shape is right. Add a citation to the exact HRS subsection because the identity using `g` and `bar(g)` is not obvious, and because "Gidney 2015 StackExchange" is not a conventional archival reference.

- Lines 52-60: modular multiplication/uncomputation

  Good, but call this Beauregard-style swap-and-inverse cleanup rather than implying it is original to HRS. The note does this later; make it explicit here too.

- Lines 68-76: Toffoli count

  Good. The source derives the controlled modular multiplier count and then `64 n^3 log_2 n + O(n^3)` Toffolis for the full Shor run.

- Lines 88-91: comparison to Takahashi/Beauregard

  Correct in asymptotic intent. The key caveat is that QFT-based adders pay rotation-synthesis overhead under fault-tolerant discrete gate sets; the semi-classical inverse QFT rotations remain but do not change the asymptotic scaling.

- Line 119: later Gidney-Ekera comparison

  Wrong by roughly an order of magnitude for RSA-2048. Gidney-Ekera's abstract-circuit formula is about `0.3 n^3 + 0.0005 n^3 lg n` Toffolis; at `n=2048` this is about `2.6e9`, not `3e8`. `3e8` is closer to the `n=1024` scale. Fix the number or remove the comparison from this source note.

- Lines 123 and 131: testability/fault localization

  Good but should be framed as a property of classical reversible Toffoli networks on computational-basis tests. It does not test arbitrary superposition behavior or the semi-classical QFT rotations.

## Missing or Suggested Additions

- Add a small parameter convention: here `n` is the bit length of the integer being factored, not the integer itself.
