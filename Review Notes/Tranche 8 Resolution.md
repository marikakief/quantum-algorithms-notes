# Tranche 8 Resolution

## Scope

Processed the Tranche 8 feedback covering early quantum chemistry and fermionic simulation: Abrams-Lloyd fermions, Ortiz/Jordan-Wigner mappings, the Aspuru-Guzik molecular-energy proposal, Kassal chemical dynamics, Lanyon's photonic H2 experiment, chemistry Trotter-error analysis, second-quantized Taylor/LCU chemistry, fermionic swap networks, and the associated trick cards.

## Implemented Corrections

- Softened Abrams-Lloyd historical claims and reframed the heap-sort issue as an incomplete coherent reversible sorting sketch, with the modern sorting-network/swap-history repair stated explicitly.
- Corrected Ortiz et al. scope: removed the Bravyi-Kitaev anachronism, defined the occupation and \(\sigma^\pm\) convention, narrowed term-by-term measurement efficiency to polynomial structure rather than practical shot efficiency, and separated operator mappings from QFT basis switching.
- Repaired Aspuru-Guzik et al. 2005 implementation details: direct mapping is now described as occupation/Fock-space mapping rather than later Pauli-string compilation, compact mapping qubits are tied to the selected configuration sector, recursive PEA starts with a 4-bit block and then refines, and ASP/gate-count claims are scoped to the paper's historical model.
- Fixed Kassal et al. 2008 arithmetic and phase-kickback details: Coulomb evaluation is \(O(B^2m^3)\) in the paper's arithmetic model, the \(d=3B-6\) coordinate count is identified as an internal-coordinate convention, Fourier-eigenstate kickback no longer claims arbitrary scratch cleanup, and BO/thermal-state/resource estimates are scoped to the paper's assumptions.
- Clarified Lanyon et al. 2010: the experiment implemented IPEA and controlled block unitaries for minimal-basis H2, while eigenstate choices, Hamiltonian reduction, ASP, and scalable Trotterization were classically precomputed or only resource-estimated.
- Scoped the Babbush-McClean-Wecker Trotter-error note to second-order, small-molecule, state-dependent chemistry error; distinguished operator-norm error, state-dependent energy bias, and phase-estimation bias; and cleaned up the state-preparation synthesis attribution.
- Tightened the second-quantized Taylor/LCU chemistry note: kept \(\Lambda\), \(s\), \(r\), and \(K=\Theta(\log(r/\epsilon)/\log\log(r/\epsilon))\) visible; labeled on-the-fly integral factorization as schematic; and described the precision improvement as logarithmic precision dependence, not an across-the-board exponential speedup.
- Corrected Kivlichan et al. 2018 and swap-network cards: the Hamiltonian is the structured \(T+U+V\) one-body plus density-density form, not generic four-index chemistry; \(N(N-1)/2\) counts logical fermionic simulation gates, not primitive native entanglers; and the naive JW comparison is cubic-scale for this Hamiltonian rather than the generic four-index chemistry estimate.
- Updated trick cards for JW, fSWAP, Trotter chemistry, plane-wave dual, first-quantized plane-wave, real-space split-operator/grid encodings, on-the-fly PREPARE, dynamic SELECT, ASP, recursive PEA, Givens Slater preparation, basis switching, parity signs, and block-diagonal grouping.

## Feedback Not Directly Implemented

- Some suggested "do not say X" feedback was retained only as corrected caveat language. For example, the notes still mention value-register cleanup and Gaussian-state scope, but only to state the precise limitation.
- I did not turn historical early-qubit estimates into modern resource estimates. They are now explicitly labeled as logical-qubit or pre-fault-tolerant estimates rather than replaced with later numbers.
- I kept broad cross-links to later chemistry notes where useful, but added access-model/input-model caveats instead of asserting a single universal best simulation representation.

## Verification

- Scoped fixed-string residual searches for the main Tranche 8 bad phrases return no matches on the target files.
- A scoped direct trailing-whitespace scan and `git diff --check` pass are clean on the Tranche 8 targets.
