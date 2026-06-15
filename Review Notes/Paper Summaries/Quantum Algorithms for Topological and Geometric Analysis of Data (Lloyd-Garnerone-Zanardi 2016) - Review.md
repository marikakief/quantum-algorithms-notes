# Review: Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016)

Source note: [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]]

## Verdict

Minor issues. This is one of the better notes in the vault: it presents the original result and the later caveats together. The main change is to keep the caveats in the theorem statement, not only after the algorithm.

## Comments

- The algorithm estimates normalized Betti numbers to additive accuracy. The note correctly says this later; put it in the computational-problem line too, so readers do not expect exact Betti numbers.

- The dependence on simplex density and spectral gap should be attached to the advertised `poly(n)` runtime. Without both promises, the original exponential-speedup statement is misleading.

- The phrase "multiplicative accuracy" should be corrected or explained. The operational estimate is additive on a normalized quantity.

- The persistent-homology section should stay modest: running across scales is not the same as computing persistent Betti numbers or barcodes.

- The QMA_1/NP-hardness discussion should distinguish exact, multiplicative, and normalized additive approximation. The note largely does this; a small table would prevent accidental conflation.

## Suggested Additions

- Add a "safe theorem statement" with all hidden parameters: simplex density, smallest nonzero Laplacian eigenvalue, qRAM/distance oracle, and normalized output.
- Add a warning that zero-eigenvalue resolution is a spectral-gap promise, not automatic from topology.
