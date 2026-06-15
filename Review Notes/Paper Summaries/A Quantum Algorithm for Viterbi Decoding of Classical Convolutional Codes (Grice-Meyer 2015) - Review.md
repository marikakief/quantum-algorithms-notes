# Review: A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015)

Source note: [[A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes (Grice-Meyer 2015) — Paper Notes]]

## Primary Sources Checked

- Grice and Meyer, "A Quantum Algorithm for Viterbi Decoding of Classical Convolutional Codes," arXiv:1405.7479.

## Verdict

Minor issues. The note is unusually careful about the algorithm's weak spots. It correctly presents QVA as an interesting coherent trellis construction rather than as a clean theorem beating classical Viterbi in ordinary regimes.

## Line-Anchored Comments

- Lines 11-31: good problem setup and comparison to classical Viterbi.
- Lines 35-41: the per-iteration gate/time distinction is important. The advertised time improvement depends on parallel execution of `|Q|` controlled transition blocks.
- Lines 57-66: good warning that the paper's example formula is notationally loose and not a normalized amplitude distribution.
- Lines 69-88: controlled transition-state preparation is described clearly.
- Lines 92-104: the proposed phase rewrite is a real improvement. A sum of phase scores is the natural implementable form; the paper's product notation invites confusion.
- Lines 108-123: transition-preparation cost accounting is useful and should stay near any complexity claim.
- Lines 127-137: the legal-path reflection/amplification step is the least rigorous part. The note correctly says the displayed operator is not a properly normalized standard reflection.
- Lines 147-153: the small examples are useful, but they are tuned demonstrations, not evidence for asymptotic advantage.
- Lines 157-173: probabilistic variant is well explained. The mode gap `b-b'` governs trial complexity, so close path probabilities are fatal.
- Lines 223-230: caveats are excellent. Keep the warning that long frames can erase the advantage through the `sqrt(F^N)` ceiling.

## Missing or Suggested Additions

- Add a "claim status" line: coherent trellis construction plus examples, not a general speedup theorem over Viterbi.
- Add a side-by-side cost line including classical Viterbi's `O(N |Q| F)` dynamic-program cost.

