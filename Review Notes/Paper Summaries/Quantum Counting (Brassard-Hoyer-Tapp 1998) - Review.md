# Review: Quantum Counting (Brassard-Hoyer-Tapp 1998)

Source note: [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]

## Primary Sources Checked

- Brassard, Hoyer, Tapp, "Quantum Counting", arXiv: https://arxiv.org/abs/quant-ph/9805082
- ArXiv metadata confirms ICALP 1998, LNCS 1443, pp. 820-831.

## Verdict

Minor issues. The note is mostly accurate, with some later terminology and theorem labeling that should be made less anachronistic.

## Line-Anchored Comments

- Lines 11 and 23: "Amplitude estimation (generalisation)"

  This is fine as modern interpretation, but in the 1998 paper the main named task is quantum counting; the fully general "amplitude estimation" terminology is more strongly associated with the 2000/2002 BHMT paper. Keep the historical distinction.

- Lines 19-23: amplitude amplification, heuristic speedup, counting

  Good match to the arXiv abstract, which explicitly describes all three.

- Lines 47-51: exact amplification

  This is plausible but should be cited more carefully. Arbitrary final phases are also closely connected to Hoyer's later arbitrary-phases paper. If the note attributes both methods to BHT 1998, include section references.

- Lines 57-61: heuristic speedup

  Good, but line 61 is too broad. The heuristic must be coherently implementable, and the cost model must include reversible implementation and checking.

- Lines 103 and 116: exact counting cost

  State the edge cases. Exact counting formulas usually need care near `t=0` and `t=N`; the later BHMT theorem uses expressions with endpoint handling. Avoid a bare `O(sqrt(tN))` as if it covers all regimes cleanly.

- Line 127: "BHMOT"

  Typo. The authors are Brassard, Hoyer, Mosca, Tapp: `BHMT`, not `BHMOT`.

## Missing or Suggested Additions

- Add a note that phase-estimation-style counting gives probability `8/pi^2` per run and needs median/majority repetition for high confidence.
