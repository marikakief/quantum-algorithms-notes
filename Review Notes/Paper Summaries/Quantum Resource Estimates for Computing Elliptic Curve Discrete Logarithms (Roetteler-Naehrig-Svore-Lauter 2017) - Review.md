# Review: Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017)

Source note: [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]

## Primary Sources Checked

- Roetteler, Naehrig, Svore, and Lauter, "Quantum resource estimates for computing elliptic curve discrete logarithms", arXiv: https://arxiv.org/abs/1706.06752
- IACR ePrint mirror: https://eprint.iacr.org/2017/598

## Verdict

Minor issues. The note is detailed and mostly source-faithful. The main problems are link normalization and a few places where probabilistic/incomplete arithmetic should be labeled more explicitly.

## Line-Anchored Comments

- Lines 35-47: headline resource numbers

  Good. The paper is precisely a concrete logical-resource estimate for prime-field ECC discrete logs, and the note correctly treats this as an arithmetic/resource-estimation paper rather than a new algorithmic speedup.

- Lines 81-83: link to Haner-Roetteler-Svore

  Check Unicode normalization. The displayed `Häner` appears to use a combining accent, while the filename may use precomposed `Häner`. Obsidian links can silently fail across normalization variants.

- Lines 89-101: table values

  These are high-value numbers; the note should say "from Table 1" or otherwise identify the source table. Without a table citation, this section is hard to audit.

- Lines 149-153: Kaliski inversion

  Good. The note captures why a fixed-round inversion routine is valuable in a coherent quantum circuit.

- Lines 181-206: incomplete group law / exceptions

  Good but should state more plainly that these are not exact complete group-addition circuits. The algorithm accepts a bounded failure/probability contribution from exceptional cases under generic assumptions.

- Lines 252-260: RSA comparison

  Useful but should not be overread as a surface-code physical estimate. This paper's comparison is at the logical circuit-resource level; later physical layout papers make different assumptions and constants.

## Missing or Suggested Additions

- Add a short glossary distinguishing field size `n`, group order bits, Toffoli count, Toffoli depth, and logical qubit count. The note has the numbers, but the parameter roles deserve a single local convention block.
