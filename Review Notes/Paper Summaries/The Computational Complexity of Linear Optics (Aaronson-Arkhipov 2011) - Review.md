# Review: The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011)

Source note: [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]]

## Verdict

Minor-to-major issues. The core BosonSampling hardness story is well explained. The main weaknesses are a few unstable/current claims and the need to keep exact, approximate, average-case, and experimental-noise regimes separate.

## Line-Anchored Comments

- Lines 9-20: The probability formula should explicitly include both input and output occupation factorials in the general collision case. Since the standard input has no repeated occupied modes, the input denominator is 1, but saying that avoids ambiguity.

- Lines 23-29: Good historical framing. Add that approximate hardness rests on conjectures plus an anti-concentration/hiding framework; exact hardness is a different theorem.

- Lines 46-54: The exact-hardness theorem should say "collapse of PH" via Toda if `P^#P` is in `BPP^NP`; the note already implies this but could be more explicit.

- Lines 56-83: The GPE/PACC/PGC chain is clearly presented. Add that Theorem 3 gives a `BPP^NP` algorithm for additive Gaussian permanent estimation assuming an approximate sampler; the #P-hardness conclusion requires the conjectures.

- Lines 89-91: The submatrix-Gaussian condition is more delicate than "any submatrix." It is for suitable random submatrices in the embedding regime, with quantitative total-variation bounds.

- Lines 122-130: "Current classical simulation frontier" is a moving 2020s claim. Either date it explicitly or remove it; otherwise this note will age quickly.

- Lines 122-130: Experimental verification is not just "orthogonal" to the complexity theory; noise can move the output distribution into classically simulable regimes. Add a sentence about noise robustness being a separate assumption.

## Missing or Suggested Additions

- Add a diagram of the approximate-hardness implication chain: sampler -> Stockmeyer -> additive GPE -> multiplicative GPE under anti-concentration -> PH collapse under PGC.
- Add a short note distinguishing BosonSampling from universal KLM linear optics.
