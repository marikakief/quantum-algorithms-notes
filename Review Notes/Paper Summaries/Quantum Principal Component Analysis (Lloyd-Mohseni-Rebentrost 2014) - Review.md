# Review: Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014)

Source note: [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes]]

## Verdict

Minor issues. The note is clear and appropriately includes Tang-style dequantization caveats. The main improvement is to keep qPCA's copy complexity, eigenvalue-resolution dependence, and data-access assumptions adjacent to every speedup claim.

## Line-Anchored Comments

- Lines 7-16: "Time `O(R log d)`" should be qualified by precision, copy complexity, and the cost of preparing/receiving copies of `rho`. The algorithm is not just logarithmic in dimension.

- Lines 31-37: Density-matrix exponentiation cost `O(t^2/epsilon)` is the right headline for the first-order partial-swap construction. Add that the copies of `rho` are consumed destructively.

- Lines 39-45: "Any density matrix is a Hamiltonian" is memorable, but it should be labeled as a simulation primitive with copy access. It does not give an efficient sparse-oracle Hamiltonian simulator for an arbitrary classically described matrix.

- Lines 63-72: The rank/precision discussion is good. State that resolving eigenvalues separated by `Delta` costs polynomially in `1/Delta`; a maximally mixed state is not useful because all eigenvalues are degenerate and small.

- Lines 75-84: The applications section should distinguish quantum-data applications from classical-data applications loaded through qRAM. qPCA is much more defensible for genuine unknown quantum states.

- Lines 87-104: The dequantization paragraph is one of the best parts of the note. Add that dequantization results require classical sample-and-query access assumptions that are strong but are the right analogue of qRAM-style access.

## Missing or Suggested Additions

- Add a resource line: copies of `rho`, controlled partial swaps, phase-estimation precision, and output sampling probability.
- Add a sentence that qPCA outputs samples/eigenvalue labels, not a full classical list of all principal components for free.
