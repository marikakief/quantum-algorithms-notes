# Review: Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekera-Hastad 2017)

Source note: [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]]

## Primary Sources Checked

- Ekera and Hastad, "Quantum algorithms for computing short discrete logarithms and factoring RSA integers", arXiv: https://arxiv.org/abs/1702.00249
- Springer/PQCrypto citation via DOI: https://doi.org/10.1007/978-3-319-59879-6_20

## Verdict

Minor issues. The note is careful about conventions and correctly identifies the RSA reduction. It needs a little more explicit separation between the original paper's convention and later Gidney-Ekera notation.

## Line-Anchored Comments

- Lines 41-49: short discrete log setup

  Good. The paper's core algorithm is for a logarithm known to be short relative to the group order, which lets one shorten one exponent register compared with the fully general Shor discrete-log algorithm.

- Line 46: `d = (p+q-2)/2`

  This is source-convention dependent and should be marked as such. Later Gidney-Ekera uses a related reduction with `d = p + q` after choosing `y = g^{N+1}`. Both can be correct under different generator/target definitions, but the note should warn readers not to identify the symbols without translating conventions.

- Lines 137-155: reduction conditions

  Good. The note correctly treats the reduction as probabilistic over random choices and not as a deterministic replacement for all order-finding.

- Lines 215-221: convention shift

  This is an excellent addition. Keep it, and perhaps move it earlier because it prevents the main confusion with the Gidney-Ekera note.

- Metadata/title

  The paper appears in PQCrypto 2017 and is also available as arXiv:1702.00249. The note should include both if it wants to use the conference-citation form.

## Missing or Suggested Additions

- Add one sentence explaining why recovering `p+q` factors RSA: once `s = p+q` is known, `p` and `q` are the roots of `X^2 - sX + N = 0`.
