# Review: Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009)

Source note: [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]

## Primary Sources Checked

- Jordan, "Fast quantum algorithms for approximating some irreducible representations of groups," arXiv:0811.0562.

## Verdict

Minor issues. The note is detailed and technically balanced. It correctly separates additive matrix-element estimation from character estimation, where classical algorithms often suffice.

## Line-Anchored Comments

- Lines 11-35: good problem statement. The word "additive" should stay in the first paragraph because multiplicative estimates of tiny matrix elements would be a very different task.
- Lines 39-43: excellent high-level assessment. Matrix elements and characters are not interchangeable computational targets.
- Lines 49-58: Hadamard-test accounting is right. Mention that off-diagonal entries require polarization/superposition tricks if this section is reused independently.
- Lines 63-83: the QFT-over-finite-groups route is clear. The inverse on `g` in the Fourier-conjugated left-regular action is a convention issue but should be kept consistent.
- Lines 87-95: the Schur transform route is accurately limited to polynomial representations appearing in tensor powers.
- Lines 98-132: the Gel'fand-Tsetlin sparse-Hamiltonian construction is the densest part. The line-100 indexing `m_{j,n+1}` is a bit compressed; consider adding a diagram or saying "interlacing between consecutive rows" in prose.
- Lines 140-153: the Young-Yamanouchi adjacent-transposition formula is clear and useful.
- Lines 166-187: the classical-character section is excellent and prevents the common overclaim that normalized characters are a quantum speedup target.
- Lines 215-236: strong caveat about typical exponentially small entries. A polynomial additive estimate can be information-poor.
- Lines 259-266: limitations are exactly right. There is no BQP-hardness theorem for these `S_n`/`A_n` matrix-element problems.

## Missing or Suggested Additions

- Add a short "precision regimes" note: polynomial additive precision is the theorem; exponentially small additive precision would erase the stated efficiency.
- Add a cross-link from the character section to later multiplicity notes, where similar `S_n` Fourier-sampling ideas also lose much of their speedup story after classical comparisons.

