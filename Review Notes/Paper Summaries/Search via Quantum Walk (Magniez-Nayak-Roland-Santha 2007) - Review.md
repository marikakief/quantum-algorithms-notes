# Review: Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007)

Source note: [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]

## Primary Sources Checked

- Magniez, Nayak, Roland, and Santha, "Search via Quantum Walk", arXiv: https://arxiv.org/abs/quant-ph/0608026
- SIAM journal DOI listed by arXiv: https://doi.org/10.1137/090745854

## Verdict

Minor issues. The note captures the MNRS contribution well. It needs cleaner treatment of stationary marked measure and a few historical/link fixes.

## Line-Anchored Comments

- Lines 9-14: problem setup

  Replace `epsilon = |M|/|X|` with marked stationary probability, or explicitly restrict to uniform stationary distribution. MNRS is formulated for a Markov chain with stationary distribution, not just a uniform vertex set.

- Lines 18-28: relation to Ambainis and Szegedy

  Good. The source abstract says the algorithm combines prior benefits: finding marked elements, smaller cost, and applicability to a larger class of chains.

- Lines 36-46: approximate reflection via phase estimation

  Good. This is the central idea: use phase estimation on the Szegedy walk to approximate reflection about the stationary state instead of paying setup cost each time.

- Lines 50-65: removing the logarithmic overhead

  Correct in spirit. The link to `[[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)]]` is risky because the reviewed vault note for that item has wrong metadata. Link to the corrected review or cite "Quantum Search on Bounded-Error Inputs", arXiv `quant-ph/0304052`.

- Lines 67-69 and 77-79: non-reversible extension

  Check the exact statement against the final version before using this as a theorem. The reversible-chain theorem is the clean main result; non-reversible cases require singular-value/discriminant conditions and should not be stated as "any ergodic chain" without qualifications.

- Lines 123-124: hitting time caveat

  The warning is valuable, but the final sentence is muddled. Szegedy's original result is usually framed through the `sqrt(delta epsilon)` rule/extended hitting behavior, while later work studies quantum hitting/finding more carefully. Rewrite this after checking the exact definitions of hitting time being compared.

## Missing or Suggested Additions

- Add the standard cost notation explicitly: `S` setup, `U` update, `C` checking, spectral gap `delta`, marked stationary measure `epsilon`.
