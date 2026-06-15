# Review: Randomized Multi-Product Sampling

Source note: [[Randomized Multi-Product Sampling]]

## Primary Sources Checked

- Faehrmann et al., arXiv:2101.07808: https://arxiv.org/abs/2101.07808

## Verdict

Needs rewrite. The card appears to omit the cross terms required to estimate an observable after a coherent multi-product approximation.

## Line-Anchored Comments

- Lines 9-11: if `U approx (1/Xi) sum C_q V_q`, then an expectation value after `U` involves a double sum over `q,q'`: `V_q^\dagger O V_{q'}`. The card's later single-index estimator cannot reproduce this in general.
- Line 18: sampling one `q` and measuring `<O>_{V_q}` estimates a mixture-like quantity, not the coherent MPF observable expectation.
- Line 21: unbiasedness is therefore not established by the protocol as written.
- Line 24: the one-ancilla Hadamard test is plausible for interference terms, but the controlled operation should involve the appropriate pair/cross term, not just controlled `V_q`.
- Lines 37-41: the "observable only" limitation is correct and important; keep it after fixing the estimator.

## Missing or Suggested Additions

- Write the target expectation as a double sum first, then give the sampling distribution and Hadamard test for the sampled term. This will make the `Xi^4` variance factor transparent.
