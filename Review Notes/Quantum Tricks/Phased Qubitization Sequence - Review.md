# Review: Phased Qubitization Sequence

Source note: [[Phased Qubitization Sequence]]

## Primary Sources Checked

- Low and Chuang QSP PRL, arXiv:1606.02685: https://arxiv.org/abs/1606.02685
- Low and Chuang qubitization paper, arXiv:1610.06546: https://arxiv.org/abs/1610.06546

## Verdict

Sound with one important cross-link caveat.

## Line-Anchored Comments

- Lines 8-12: the idea is right: the phase list is the programmable part once the signal has been encoded.
- Lines 18-20: the caveat about phase synthesis is important and should stay. It prevents the common mistake of treating QSP as only an asymptotic theorem.
- Line 27: the Babbush-Gidney chemistry application primarily uses qubitized phase estimation on the walk operator for spectroscopy. Do not imply that its phase-estimation step necessarily uses Jacobi-Anger QSP Hamiltonian-simulation truncation unless a specific source section says so.
- Line 28: the SYK/Airy-function note is interesting but downstream; keep it clearly separated from the core Low-Chuang sequence.

## Missing or Suggested Additions

- Add the parity/degree constraints on the polynomial implemented by a length-`d` phase sequence.
