# Review: A Quantum Algorithm for Finding the Minimum (Durr-Hoyer 1996)

Source note: [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]

## Primary Sources Checked

- Durr and Hoyer, "A quantum algorithm for finding the minimum", arXiv: https://arxiv.org/abs/quant-ph/9607014
- ar5iv rendering of the paper: https://ar5iv.labs.arxiv.org/html/quant-ph/9607014

## Verdict

Minor issues. The note captures the algorithm and proof idea well. The main fixes are attribution/parameter precision and one stale link.

## Line-Anchored Comments

- Line 11: lower bound attribution

  The `Omega(sqrt(N))` lower bound is right, but calling it "the BBBV lower bound" is too compressed. Durr-Hoyer cite Bennett-Bernstein-Brassard-Vazirani as a general lower bound; for minimum finding, the clean statement is that the problem contains unstructured search as a special/reduction case.

- Lines 15 and 26: use of exponential search

  Correct. The source explicitly says the main subroutine is the Boyer-Brassard-Hoyer-Tapp quantum exponential search algorithm, a generalization of Grover search.

- Lines 26 and 48: timeout constants

  Good to include, but the note should say these constants are from the paper's proof conventions, not universal constants for all implementations of minimum finding. The algorithm's durable statement is `O(sqrt(N))` queries with constant success probability.

- Lines 40-46: expected-time analysis

  Essentially right. The paper's Lemma 1 says the probability of ever choosing an element of rank `r` is `1/r`, and Lemma 2 uses this to bound the infinite algorithm's expected time. The note should be a bit more careful than "telescopes"; it is a weighted summation using the exponential-search cost.

- Line 57: boosted success probability

  Correct if the repetitions are independent and one classically takes the best returned value. The paper also notes that one longer run can be better because it keeps threshold improvements from earlier steps.

- Line 100: broken/ambiguous link

  `[[Amplitude Amplification and Estimation]]` does not match the reviewed BHMT paper note title in this vault. Link to `[[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]` or to `[[Standard Amplitude Amplification]]`.

## Missing or Suggested Additions

- Add a one-sentence model statement: table values are accessed by a comparison oracle that can mark `T[j] < T[y]`, and the threshold `y` is classical between Grover calls.
