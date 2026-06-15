# Review: Quantum Basin Hopping with Gradient-Based Local Optimisation (Bulger 2005)

Source note: [[Quantum Basin Hopping with Gradient-Based Local Optimisation (Bulger 2005) — Paper Notes]]

## Primary Sources Checked

- Bulger, "Quantum basin hopping with gradient-based local optimisation," arXiv:quant-ph/0507193 (2005).

## Verdict

Minor issues. The note is appropriately skeptical and explains the three-region error analysis well. The main addition should be a sharper statement of the oracle assumptions behind "one-query" gradient descent.

## Line-Anchored Comments

- Lines 11-19: good problem framing. The phrase "one coherent function query" should be tied to Jordan-style phase-oracle access and adequate arithmetic precision.
- Lines 23-25: strong summary. The paper is mainly about preserving Grover-style basin search under coherent gradient-estimation errors, not about making nonconvex optimization easy.
- Lines 31-44: the GBS routine is clear. Add that the local-search path must be implemented reversibly and then uncomputed; this is a major modeling assumption.
- Lines 50-65: the accumulated large-error bound is useful. The note should stress that the gradient-estimation precision must shrink as the number of Grover/local-search calls grows.
- Lines 79-107: the three-region analysis is the best part of the note and should be retained if summarized elsewhere.
- Lines 95-107 and 128-130: the condition `N_beta << N_gamma` is central. If the threshold lies near many basin boundaries, the Grover rotation can fail; this is not a small technicality.
- Lines 111-114: "finds a basin" should be read as a conditional Grover-style scaling result, assuming the basin predicate is coherent and the boundary set is small.
- Lines 126-134: the caveats are good and proportionate.

## Missing or Suggested Additions

- Add a one-line historical context: this is best viewed as early query-model plumbing for coherent local search, not as a competitive continuous-optimization algorithm.

