# Review: Grover Search with Evolving Threshold for Minimum Finding

Source note: [[Grover Search with Evolving Threshold for Minimum Finding]]

## Primary Sources Checked

- Durr and Hoyer, "A quantum algorithm for finding the minimum", arXiv: https://arxiv.org/abs/quant-ph/9607014
- ar5iv rendering: https://ar5iv.labs.arxiv.org/html/quant-ph/9607014

## Verdict

Minor issues. The trick is right, but the displayed summation is heuristic and should not be presented as the proof.

## Line-Anchored Comments

- Lines 6 and 12: core idea

  Correct. The threshold is updated classically, and the marking oracle changes to mark indices below the current threshold.

- Lines 14-18: why total cost is `O(sqrt(N))`

  The intuition is good, but the displayed sum is not the source proof. Durr-Hoyer use a rank-visitation probability lemma (`1/r`) and bound the expected total exponential-search time. Replace the informal `sum_t 1/sqrt(t)` line or label it explicitly as intuition.

- Line 20: timeout and success probability

  Correct. The paper obtains constant success probability from an `O(sqrt(N))` timeout and then boosts by repetition.

- Lines 27-31: complexity and caveat

  Good. Coherent comparison-oracle access is required; sample access is not enough.

- Line 36: related link

  `[[Amplitude Amplification and Estimation]]` is probably stale or ambiguous in this vault. Link to `[[Standard Amplitude Amplification]]` or the full BHMT paper note.

## Missing or Suggested Additions

- Mention that the algorithm handles non-distinct values with essentially the same bound; the source explicitly notes the lemma changes from equality to inequality.
