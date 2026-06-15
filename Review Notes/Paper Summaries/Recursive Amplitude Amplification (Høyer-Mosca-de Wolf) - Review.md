# Review: Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)

Source note: [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)]]

## Primary Sources Checked

- Høyer, Mosca, and de Wolf, "Quantum Search on Bounded-Error Inputs," ICALP/STOC-era version, arXiv:quant-ph/0209054.

## Verdict

Major omissions. The source note is explicitly a stub, and it should remain marked that way until the recursion construction, parameter conventions, and bounded-error search theorem are filled in.

## Line-Anchored Comments

- Line 1: good that the note is visibly marked as a stub.
- Lines 17-22: the key result is too ambiguous. If `a` denotes success amplitude, `O(1/a)` may correspond to `O(1/sqrt(p))` for success probability `p`; if `a` denotes success probability, this would be wrong. Define the parameter.
- Lines 19-21: "removes the log overhead" needs the exact setting: search/amplitude amplification when the checking oracle has bounded error, not ordinary exact-reflection amplitude amplification.
- Lines 25-28: the role in MNRS is plausible and important. Add the theorem statement from quantum search on bounded-error inputs before using it as a dependency in walk-search notes.
- Lines 31-35: the TODO list is accurate but too short. It should also include success-probability amplification, recursive depth, and comparison with naive repetition.

## Missing or Suggested Additions

- Add the actual bounded-error input model: the checking procedure marks good items with high probability and bad items with low probability, and recursive amplification avoids paying a logarithmic reliability boost at every search level.
- Add the final query theorem and parameter names exactly as in the paper.

