# Review: A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004)

Source note: [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]

## Primary Sources Checked

- Regev, "A subexponential time algorithm for the dihedral hidden subgroup problem with polynomial space", arXiv:quant-ph/0406151 (2004).

## Verdict

Minor issues. The note captures the space/time tradeoff well. The main improvement is to be a little more precise about which space is polynomial and where the classical exponential-in-`l` enumeration enters.

## Line-Anchored Comments

- Lines 9-15: good problem comparison to Kuperberg. Use `n = log N` consistently after this point.
- Lines 15-17: the headline tradeoff is correct: `2^{O(sqrt(log N log log N))}` time/query and polynomial space.
- Lines 27-28: the recap of Kuperberg piles is useful, but "each clearing `k` bits" should be checked against the exact staged parameterization. It is fine as intuition, but not as a formal statement.
- Lines 31-48: the batch-combination construction is well explained. The count of matching bit strings and the constant-success window are the main technical point.
- Lines 50-54: good pipeline summary. Make clear that `2^{O(l)}` enumeration is classical work inside each batch attempt, not quantum memory.
- Lines 59-61: theorem statement is accurate.
- Lines 67-71: the comparison table is useful. "No piles" does not mean no repeated regeneration; it means no stored exponential list of quantum states.
- Lines 79-83: caveats are good. The brute-force enumeration warning should be promoted because it is where the extra `sqrt(log n)` factor comes from.

## Missing or Suggested Additions

- Add a one-line explanation that Regev's algorithm is often preferable in cryptographic estimates when quantum memory is treated as much more expensive than classical time.
- Cross-link this note from the Kuperberg 2013 review as one endpoint of the collimation tradeoff family.

