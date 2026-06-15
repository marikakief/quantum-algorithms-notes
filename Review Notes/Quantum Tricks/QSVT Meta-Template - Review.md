# Review: QSVT Meta-Template

Source note: [[QSVT Meta-Template]]

## Primary Sources Checked

- Gilyen, Su, Low, Wiebe, arXiv:1806.01838: https://arxiv.org/abs/1806.01838

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 8-12: the template is correct. Add that the polynomial must satisfy QSVT's parity and boundedness constraints; otherwise there may be no phase sequence.
- Lines 20-23: degree is a main query driver, but not the only complexity driver. Block-encoding normalization, block-encoding implementation cost, success probability, and error budget often dominate.
- Lines 25-27: stochastic QSP is interesting downstream context, but it is much newer than the source. Label this explicitly as a variant, not part of the 2018/2019 QSVT paper's core template.

## Missing or Suggested Additions

- Add "normalize first, approximate second" as a rule. Many mistakes in this vault come from forgetting that QSVT acts on singular values in `[-1,1]`.
