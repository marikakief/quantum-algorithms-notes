# Review: A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005)

Source note: [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]

## Primary Sources Checked

- Kuperberg, "A subexponential-time quantum algorithm for the dihedral hidden subgroup problem", arXiv:quant-ph/0302112 / SIAM Journal on Computing 35 (2005).

## Verdict

Major issues. The core labelled-qubit sieve is explained well, but the note contains a serious false statement about the later 2013 Kuperberg paper and a suspicious complexity expression for the smooth-modulus variant.

## Line-Anchored Comments

- Lines 9-15: good DHSP/hidden-shift setup.
- Lines 19-25: the high-level explanation is strong. The character-transform labelled-qubit viewpoint is the right way to teach the algorithm.
- Lines 29-41: the qubit-combination derivation is accurate, up to missing normalization.
- Lines 47-52: the power-of-two sieve outline is useful. Clarify that the retained measurement outcome/sign convention is part of the sieve analysis; the failed outcomes are not simply free.
- Lines 54 and 64: the expression `e^{O(\sqrt[3]{2\log^3 N})}` simplifies to `e^{O(\log N)}` if read literally, so it cannot be the intended subexponential smooth-case improvement. Recheck the theorem statement before keeping this formula.
- Lines 62-66: the main theorem `2^{O(sqrt(log N))}` is correct.
- Lines 72-79: the comparison table is good. Query, time, and space should stay separate.
- Lines 83-87: line 87 is wrong. Kuperberg's later "Another subexponential-time..." paper does not give a `2^{O(cuberoot(log N))}` algorithm; it gives a collimation/tradeoff approach with small quantum space and subexponential time at the same general `exp(O(sqrt(log N)))` scale. Also the arXiv identifier here appears wrong; the 2013 note in this vault is arXiv:1112.3333.
- Lines 95-106: the references are mostly good, but update the Kuperberg 2013 relation after fixing line 87.

## Missing or Suggested Additions

- Add a short resource paragraph distinguishing Kuperberg 2005's quantum space from Regev 2004's polynomial-space variant and Kuperberg 2013's classical/quantum-space tradeoffs.
- If keeping Algorithm 3 for smooth `N`, quote the paper's exact asymptotic in a notation that is visibly subexponential in `log N`.

