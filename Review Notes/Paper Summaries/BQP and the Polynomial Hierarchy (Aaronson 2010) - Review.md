# Review: BQP and the Polynomial Hierarchy (Aaronson 2010)

Source note: [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]]

## Primary Sources Checked

- Aaronson, "BQP and the polynomial hierarchy," arXiv:0910.4698; STOC 2010.
- Later context checked from the note's cited follow-ups: Aaronson-Ambainis Forrelation work, Tal's generalized Linial-Nisan result, and Raz-Tal's oracle separation programme.

## Verdict

Minor issues. The note is strong and historically well contextualized. The main improvement is to be more cautious when summarizing later programme-completion claims, because several follow-up results have slightly different formulations.

## Line-Anchored Comments

- Lines 9-16: good definitions of Fourier Checking and Fourier Fishing.
- Lines 21-31: the three-result summary is accurate. The relation-vs-decision distinction is important and should stay.
- Lines 29-31: good modern context, but cite the later results precisely if this is used in a timeline. Tal/Raz-Tal results vindicate the programme, but the exact statements are not identical to the 2010 conjectural theorem word-for-word.
- Lines 39-45: Fourier Fishing quantum algorithm is clear. The probability distribution is Fourier-coefficient squared, which is the right sampling intuition.
- Lines 48-59: Fourier Checking circuit is accurate and matches the later Forrelation primitive.
- Lines 65-73: the secret-bias/Majority explanation is good, but it is a high-level sketch. The PH-to-AC0 translation hides oracle-size and depth bookkeeping.
- Lines 75-80: almost `k`-wise independence and the Generalized Linial-Nisan Conjecture are correctly identified as the technical bottleneck.
- Lines 83-92: key result table is useful. Preserve the "conditional" label on the decision separation in this paper.
- Lines 110-119: the programme-completed section is helpful, but avoid saying the original paper itself proved these later facts.
- Lines 123-128: caveats are good. In particular, all separations are oracle/promise/relation statements, not an unconditional separation of standard complexity classes.

## Missing or Suggested Additions

- Add a "2010 statement vs later theorem" note: conditional BQP-vs-PH oracle separation here; unconditional oracle separation arrived later.
- Add a cross-link to the Aaronson-Ambainis Forrelation note as the optimized query-complexity version.

