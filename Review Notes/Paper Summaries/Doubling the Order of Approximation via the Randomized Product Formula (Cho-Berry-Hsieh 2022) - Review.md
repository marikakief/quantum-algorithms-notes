# Review: Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022)

Source note: [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]]

## Primary Sources Checked

- Cho, Berry, Hsieh, "Doubling the order of approximation via the randomized product formula," arXiv:2210.11281: https://arxiv.org/abs/2210.11281

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 13-19: the headline order-doubling result and Pauli-string implementation caveat are accurate and important.
- Lines 27-29: the time-reversibility explanation is good, but it would help to explicitly distinguish approximation order from error order. The paper's title language can be confusing.
- Lines 41-47: the correction-unitary construction is dense. Add a short intuitive sentence: the randomized correction has the desired leading generator only in expectation.
- Line 83: "improvement ... in all parameters simultaneously" is too strong. The method outputs a randomized channel, requires implementable correction terms, and is not a coherent-unitary drop-in replacement.
- Lines 87-95: the limitations section is strong and should stay.

## Missing or Suggested Additions

- Add an explicit warning that this is useful for Pauli-string Hamiltonians but not automatically for oracle-access sparse Hamiltonians.
