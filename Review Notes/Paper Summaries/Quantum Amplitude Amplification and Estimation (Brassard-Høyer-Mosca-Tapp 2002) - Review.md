# Review: Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002)

Source note: [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]

## Primary Sources Checked

- Brassard, Høyer, Mosca, and Tapp, "Quantum Amplitude Amplification and Estimation," Contemporary Mathematics 305 (2002), arXiv:quant-ph/0005055.

## Verdict

Minor-to-major issues. The note is readable and captures the two core primitives, but it incorrectly states exact success for the basic known-amplitude iterate and gives an endpoint-unsafe exact-counting complexity.

## Line-Anchored Comments

- Lines 9-13: good high-level summary. Add that amplitude estimation's `O(1/epsilon)` is for coherent uses of `A`, `A^{-1}`, and controlled/reflected Grover iterates.
- Lines 21-24: setup is clear. Mention that `A` is taken unitary/no intermediate measurements after purification.
- Lines 37-45: the 2D rotation formula is right.
- Line 47: this is the main technical problem. With `m=floor(pi/(4 theta_a))`, standard amplitude amplification gives high success, not certainty in general. Certainty requires the exact-amplification phase-adjustment machinery, which should be separated from the basic theorem.
- Lines 49-50: unknown-`a` search by exponentially increasing guesses is right, but the cost is expected and bounded-error unless extra repetition is used.
- Lines 61-67: the amplitude-estimation theorem is accurately stated for the usual success probability `8/pi^2`.
- Lines 75-80: the exact-counting row is not endpoint-safe. `O(sqrt(t(N-t)))` cannot be the full story at `t=0` or `t=N`; include the `+1`/special-case handling used in precise statements.
- Lines 86-93 and 114: "any classical randomized algorithm" is too broad. The heuristic must be coherently implementable and have a recognizable success predicate; costs of reversibilization and checking success are part of `A`.
- Lines 101-104: the relationship-to-later-work table is useful, especially the QSVT connection.

## Missing or Suggested Additions

- Add a small "three variants" box: standard high-probability amplification, exact amplification with modified phases, and amplitude estimation via phase estimation.

