# Review: Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011)

Source note: [[Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011) — Paper Notes]]

## Primary Sources Checked

- Belovs and Lee, "Quantum Algorithm for k-distinctness with Prior Knowledge on the Input", arXiv: https://arxiv.org/abs/1108.3022
- Belovs, "Learning-Graph-Based Quantum Algorithm for k-distinctness", arXiv: https://arxiv.org/abs/1205.1534

## Verdict

Minor issues. The note is unusually careful about the promise. It should add a later-update caveat: Belovs's 2012 paper removed the prior-knowledge assumption for the same exponent.

## Line-Anchored Comments

- Lines 20-22: promise on lower-order tuple counts

  Excellent. This is the central caveat of the 2011 paper and the note states it clearly.

- Lines 28-30 and 34-40: exponent

  Correct for fixed `k`: `O(n^{1 - 2^{k-2}/(2^k - 1)})`; for `k=3`, this is `O(n^{5/7})`.

- Lines 42-44: learning-graph interpretation

  Good. The note correctly emphasizes that the contribution is the value-dependent learning-graph/adversary construction, not a cleaner Johnson-graph walk.

- Lines 72-82: learning graph to adversary

  Broadly correct. Since the note writes `Adv^{±}(f) <= C`, add one sentence explaining that this is a feasible dual-adversary upper bound which, by the tight adversary characterization, yields an algorithm of comparable query complexity.

- Lines 127-145: complexity expression and balancing

  Good. This is the right high-level cost formula.

- Lines 181 and 188: "did not solve ordinary k-distinctness"

  True for this 2011 paper, but incomplete in a present-day review vault. Belovs 2012 gives the same exponent without prior information and with simpler analysis. Add a "later update" paragraph so readers do not leave thinking the promise remained necessary.

## Missing or Suggested Additions

- Add a comparison row for Belovs 2012, distinguishing this paper's promised result from the later unconditional learning-graph algorithm.
