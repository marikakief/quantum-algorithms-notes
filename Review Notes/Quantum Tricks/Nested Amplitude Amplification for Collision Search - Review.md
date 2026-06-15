# Review: Nested Amplitude Amplification for Collision Search

Source note: [[Nested Amplitude Amplification for Collision Search]]

## Primary Sources Checked

- Buhrman et al., "Quantum Algorithms for Element Distinctness", arXiv: https://arxiv.org/abs/quant-ph/0007016
- Brassard, Hoyer, and Tapp, "Quantum Algorithm for the Collision Problem", arXiv: https://arxiv.org/abs/quant-ph/9705002

## Verdict

Minor issues. The card is accurate for the 2001 algorithm. It should avoid saying the logarithmic factor is inherent in any absolute sense.

## Line-Anchored Comments

- Lines 8 and 12-19: algorithmic structure

  Correct. Random subsets, sorting, inner Grover search, and outer amplification are the paper's core pattern.

- Line 17: success probability

  Good for the one-collision/element-distinctness intuition. For general claw finding with `N <= M`, keep the asymmetric `N,M,l` expression from the paper note.

- Lines 30-32: complexity

  Correct: `O(N^{3/4} log N)` comparisons for ED and `O(N^{1/2} M^{1/4} log N)` in the specified claw-finding range.

- Line 36: space comparison

  Good. This algorithm's lower space can be a reason to care despite worse query complexity than Ambainis's optimal Johnson walk.

- Line 38: "log factor ... can't be removed"

  Too absolute. It is inherent to this sorting/binary-search implementation in the comparison-model presentation, but different models or different algorithmic structures can remove or change the log factor.

## Missing or Suggested Additions

- Add a direct comparison to BHT: BHT's `N^{1/3}` relies on the `r`-to-one promise; this nested-amplification method handles ordinary element distinctness without that promise.
