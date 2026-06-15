# Review: Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003)

Source note: [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]

## Primary Sources Checked

- Cheng, "Primality Proving via One Round in ECPP and One Iteration in AKS", arXiv:math/0301179 / Journal of Cryptology 20 (2007).

## Verdict

Minor issues. The note is strong and appropriately skeptical about the heuristic runtime. The most important editorial point is to keep "proved certificate criterion" separate from "heuristic expected prover time".

## Line-Anchored Comments

- Lines 13-17: good framing as classical primality proving rather than quantum algorithms.
- Lines 21-31: the heuristic dependencies are identified correctly. Do not soften this; the quartic expected time is not a theorem.
- Lines 37-49: the `C`-good definition is clear. It would help to say `r` is a prime factor of `n-1`, not the AKS order parameter used in the original AKS paper.
- Lines 53-81: the one-congruence certificate is well described. The condition `a != 1` and `a^(r^{alpha-1}) != 1` is not just a random-base convenience; it forces the relevant `r`-power order needed for irreducibility.
- Lines 83-103: good ECPP reduction description. Stress that the ECPP stage must find a nearby `n'` that is both probable prime and good; this is the source of the short-interval heuristic.
- Lines 125-157: the proof sketch is good. The subgroup-size versus automorphism pigeonhole argument is the nontrivial AKS-style core.
- Lines 159-179: excellent caveat that the Mertens argument counts integers, not primes in the Hasse interval.
- Lines 181-198: label the assumptions as a list of assumptions for prover time; certificate verification remains deterministic once the certificate data exist.
- Lines 214-217: good memory warning. The `2^30`-bit intermediate is a useful practical sanity check and should stay.

## Missing or Suggested Additions

- Add a small note that Rabin-Miller in the algorithm is used as a compositeness witness finder in the randomized prover, not as an assumption in the certificate verifier.
- In the comparison table, separate ECPP certificate-checking time from ECPP certificate-generation time, since the point of the paper is partly that tradeoff.

