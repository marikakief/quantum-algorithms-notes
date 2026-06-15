# Review: Recursive Amplitude Amplification (Hoyer-Mosca-de Wolf)

Source note: [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)]]

## Primary Sources Checked

- Hoyer, Mosca, de Wolf, "Quantum Search on Bounded-Error Inputs", arXiv: https://arxiv.org/abs/quant-ph/0304052
- ArXiv metadata confirms ICALP 2003, LNCS 2719, pp. 291-299.

## Verdict

Needs rewrite. The current note is a stub and has incorrect bibliographic metadata.

## Line-Anchored Comments

- Lines 9-13: metadata

  Wrong arXiv and venue. `quant-ph/0209054` is an unrelated PT-symmetry paper. The relevant paper is `quant-ph/0304052`, titled "Quantum Search on Bounded-Error Inputs", appearing at ICALP 2003, not STOC 2003.

- Lines 17-22: key result

  The high-level idea is close: recursive interleaving of amplitude amplification and error reduction avoids first reducing all bounded-error inputs to inverse-polynomial error. But the statement "exact amplitude amplification in `O(1/a)` queries" is not the right formulation. The arXiv abstract says `O(sqrt(n))` repetitions of the base algorithms suffice to find an index of a 1-bit among `n` bounded-error algorithms, removing an `O(log n)` overhead.

- Lines 25-27: role in MNRS

  This is plausible, but should be rewritten with the actual bounded-error-input search theorem and then explain how approximate walk reflections/verifiers fit the theorem.

- Lines 31-34: TODO

  Keep these, but add "fix source metadata" as the first TODO.

## Missing or Suggested Additions

- Rename or retitle the note to make clear it reviews Hoyer-Mosca-de Wolf's bounded-error input search, not a generic recursive amplitude-amplification theorem.
