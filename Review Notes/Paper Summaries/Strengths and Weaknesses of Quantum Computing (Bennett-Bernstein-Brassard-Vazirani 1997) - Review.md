# Review: Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997)

Source note: [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]]

## Verdict

Minor issues. The BBBV search lower bound and tidy-subroutine construction are explained well. The main fixes are to keep the random-oracle statements distinct from the black-box search theorem and to be precise about `NP cap coNP` under permutation oracles.

## Line-Anchored Comments

- Lines 17-27: The three-result summary is good. Add that the search lower bound is a query lower bound, while the NP oracle separations are almost-sure statements relative to random oracles.

- Lines 41-61: The query-magnitude hybrid argument is clearly written. It would benefit from one sentence connecting Euclidean distance of final states to bounded variation distance of measurement outcomes.

- Lines 63-70: The counting argument is useful, but make clear that it proves an oracle separation and does not rule out structured algorithms for NP-complete problems.

- Lines 74-80: The permutation-oracle lower bound should define the promise problem used for `NP cap coNP`; "membership" is too vague.

- Lines 84-98: The tidy-subroutine section is important. Add that this is a coherent simulation of a bounded-error decision subroutine after amplification, not arbitrary black-box access to its internal randomness.

- Lines 114-123: The lower-bound-method comparison is good. The negative-weight adversary line should say it characterizes bounded-error quantum query complexity up to constants.

## Missing or Suggested Additions

- Add Boyer-Brassard-Hoyer-Tapp's exact constants as a side note if the vault has a Grover/amplitude-amplification thread.
- Add a small "oracle vs unrelativized" warning near the title or first paragraph.
