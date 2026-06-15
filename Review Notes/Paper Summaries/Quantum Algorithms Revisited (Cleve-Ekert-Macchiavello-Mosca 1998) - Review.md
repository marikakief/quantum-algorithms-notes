# Review: Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998)

Source note: [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]]

## Primary Sources Checked

- Cleve, Ekert, Macchiavello, Mosca, "Quantum Algorithms Revisited", arXiv: https://arxiv.org/abs/quant-ph/9708016
- Royal Society DOI listed by arXiv: https://doi.org/10.1098/rspa.1998.0164

## Verdict

Major issues. The explanatory framing is good, but several statements overclaim what the paper "introduced" and one QFT formula is missing normalization.

## Line-Anchored Comments

- Lines 9-17: "Introduces the general phase estimation algorithm explicitly"

  Soften this. The paper reviews and crystallizes algorithms as phase estimation/interference, building on Kitaev. It should not be presented as the original source of phase estimation itself.

- Lines 52-55: QFT state product formula

  The displayed product state is missing normalization factors. Each factor should be normalized, e.g. `(|0> + e^{...}|1>)/sqrt(2)`, or the whole expression needs the `2^{-m/2}` prefactor.

- Lines 79-81: success probability and extra bits

  The broad `4/pi^2` statement is standard, but the exact extra-bit formula should be checked against the paper's Appendix C notation. If retained, state which symbols correspond to "desired output bits" and "total phase-estimation register bits".

- Lines 83-87: "`|1>` trick" and two independent runs

  This is too strong. In Shor/order finding, a single sampled eigenphase `k/r` recovers `r` only when `gcd(k,r)=1`; otherwise it yields a denominator dividing `r`, and multiple samples are combined. The claim that two independent runs give coprime `k_1,k_2` with probability `>0.54` is not the right general success guarantee for recovering an arbitrary `r`.

- Lines 91-93: "This paper gives the deterministic version that's now standard"

  The deterministic Deutsch-Jozsa version predates this paper. This paper reviews/improves and presents the phase-kickback/interferometry view. Do not imply the exact Deutsch-Jozsa algorithm originates here.

- Line 113: "All known quantum algorithms"

  Too broad even historically. The arXiv abstract says a common pattern underpinning quantum algorithms can be identified, not that every known algorithm is literally phase estimation.

## Missing or Suggested Additions

- Add a "Review paper, not invention paper" caveat near the top.
- Add a normalization correction for the QFT formula before this note is reused by other QFT cards.
