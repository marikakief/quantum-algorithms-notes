# Review: Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on)

Source note: [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes]]

## Primary Sources Checked

- Babbush, Berry, Kothari, Somma, and Wiebe, "Exponential quantum speedup in simulating coupled classical oscillators," Physical Review X 13, 041041 (2023), arXiv:2303.13012.
- Detailed vault companion review: [[Review Notes/Paper Summaries/Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) - Review]].

## Verdict

Minor-to-major issues due to duplication and metadata confusion. The short note says many correct high-level things, but it is redundant with the more detailed oscillator summary and its filename/reference framing is misleading.

## Line-Anchored Comments

- Lines 1-3: the source is the 2023 PRX/arXiv:2303.13012 oscillator paper. The filename phrase "Quantum 2020-04-20-254 follow-on" is confusing and should not be used as the primary identifier.
- Lines 7-10: the one-line take is basically right, provided "global observables" and "oracle/query separation" stay in the sentence.
- Lines 15-19: the block-Hamiltonian lifting sketch is accurate at the level of a short duplicate note.
- Lines 25-29: the complexity headline is too compressed. The exponential lower bound is an oracle lower bound inherited through welded trees, while BQP-completeness applies when the oracle family is efficiently generated; these are related but different claims.
- Lines 34-36: good caveat. This should appear before the headline, not after it.
- Lines 51-54: caveats are strong and match the detailed review.
- Line 60: the link to "Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254)" looks suspect as a "used here" reference. Verify whether this is actually a cited/used technique in arXiv:2303.13012 or just a nearby Hamiltonian-simulation paper.
- Line 62: this cites "Babbush et al. (2023)" as related/concurrent even though that is the same source named in line 1.
- Lines 77-78: the duplicate warning is useful. This note should either be merged into the detailed note or kept only as a stub redirect.

## Missing or Suggested Additions

- Add an explicit "duplicate/redirect" status at the top and point readers to the detailed 2023 oscillator note.
- If retained, remove ambiguous 2020 metadata and keep only the correct PRX 2023/arXiv:2303.13012 citation.

