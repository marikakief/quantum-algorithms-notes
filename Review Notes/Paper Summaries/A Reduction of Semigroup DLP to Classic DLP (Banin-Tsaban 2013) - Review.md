# Review: A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013)

Source note: [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]]

## Primary Sources Checked

- Banin and Tsaban, "A reduction of semigroup DLP to classic DLP", arXiv:1310.7903 (2013).

## Verdict

Minor issues. The note is mostly sound and nicely paired with Childs-Ivanyos. The key thing is to keep the result scoped to one-generator periodic semigroups with good encodings and a group-DLP oracle.

## Line-Anchored Comments

- Lines 11-27: good definition of periodic semigroup DLP and the tail/cycle parameters.
- Lines 29-35: the "not a post-quantum escape hatch" conclusion is appropriate under the encoding and multiplication assumptions.
- Lines 39-56: the cyclic group inside the eventual cycle is accurately described. The identity `g^{tn}` and generator `g^{tn+1}` are the right objects.
- Lines 60-78: the random order-recovery section is plausible but compressed. The note should say that the random exponent range must be large enough to wash out the tail and approximate uniformity over the cycle.
- Lines 80-89: the idempotent boundary search is clear and useful.
- Lines 93-121: good split between `h in G` and `h notin G`. The second binary search is the easiest part to lose; keep it.
- Lines 125-134: excellent warning that the infinite-order section is heuristic rather than a theorem.
- Lines 148-151: the note already says taking an lcm is safer than a maximum; that implementation detail should be moved up near the algorithm description.
- Lines 167-172: good comparison with Childs-Ivanyos. Keep the distinction between a classical oracle reduction and a direct quantum algorithm.

## Missing or Suggested Additions

- Add a short sentence saying the reduction does not make cyclic group DLP easier; it only shows the semigroup wrapper adds no extra hardness in the periodic one-generator case.
- The source note should be careful with "classic DLP": use "ordinary cyclic-group DLP" to avoid suggesting a purely classical algorithm without an oracle.

