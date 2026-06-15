# Review: Quadratic Speedup for Classical Heuristics via Amplitude Amplification

Source note: [[Quadratic Speedup for Classical Heuristics via Amplitude Amplification]]

## Primary Sources Checked

- BHT quantum counting abstract: https://arxiv.org/abs/quant-ph/9805082
- BHMT amplitude amplification abstract: https://arxiv.org/abs/quant-ph/0005055

## Verdict

Major issues. The core result exists, but the opening overstates it as applying to any probabilistic algorithm.

## Line-Anchored Comments

- Line 8: "any classical probabilistic algorithm"

  Too broad. The source result is about search heuristics with checkable good outputs that can be run coherently/reversibly. It is not a generic square-root speedup for arbitrary randomized algorithms.

- Lines 12-20: seed-count proof

  This is the right model: random seeds, good-seed count, and coherent evaluation. Keep this, and make line 8 match it.

- Line 26: "SAT solvers, local search, simulated annealing restarts"

  These are plausible only after choosing a bounded-time, reversible, coherently seeded version with a clean success predicate. Real-world adaptive solvers with pruning, restarts, memory, and data-dependent runtime need careful reversible accounting.

- Line 36: caveat

  Good. Move this caveat earlier because it is central, not a footnote.

## Missing or Suggested Additions

- Add "Las Vegas/variable-time algorithms may need variable-time amplitude amplification or explicit runtime bucketing" if the card will be used outside fixed-runtime heuristic settings.
