# Review: Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001)

Source note: [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]]

## Primary Sources Checked

- Buhrman, Dürr, Heiligman, Høyer, Magniez, Santha, and de Wolf, "Quantum algorithms for element distinctness," SIAM Journal on Computing 34(6), 1324-1330 (2005), arXiv:quant-ph/0007016.

## Verdict

Minor issues. The note is historically well positioned and correctly distinguishes this `N^{3/4}` amplitude-amplification algorithm from Ambainis's later `N^{2/3}` quantum walk algorithm. The main improvements are around comparison-vs-query model precision.

## Line-Anchored Comments

- Lines 9-13: good problem statement. Keep the comparison model visible; many readers will otherwise assume ordinary value-oracle queries.
- Lines 19-21: the historical note is right. This paper is no longer algorithmically best, but it is a clean pre-walk construction.
- Lines 31-39: the claw-finder cost analysis is clear. The success probability assumes the relevant claw is distributed into the sampled subsets; say whether this is for a unique claw or after reduction to a promised case.
- Lines 41-48: the `ell=sqrt(N)` specialization for element distinctness is correct for the stated bound.
- Lines 51-52: the "recovering BHT" paragraph is useful, but no outer amplification being needed depends on the structured `2`-to-`1` promise and random sampling avoiding immediate table collisions.
- Lines 57-69: theorem table is good. For triangle finding, state the graph access model if this note is reused.
- Lines 75-84: good comparison table. The BHT row should be visually separated as "structured collision," not an element-distinctness algorithm for arbitrary inputs.
- Lines 92-99: lower-bound discussion is accurate and useful. Keep the later Aaronson-Shi lower bound clearly marked as later work.
- Lines 117-120: caveats are strong. The log factors and weak lower bound are exactly the right caveats for this paper.

## Missing or Suggested Additions

- Add a short model line for each problem: comparison oracle, value oracle, and graph oracle variants have slightly different overheads.

