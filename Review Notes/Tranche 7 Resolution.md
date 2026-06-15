# Tranche 7 Resolution

## Scope

Processed the Tranche 7 feedback covering modern product-formula refinements: commutator-aware Trotter bounds, qDRIFT and randomized ordering, lattice product formulas, well-conditioned and randomized multiproduct formulas, randomized order doubling, commutator product formulas, corrected product formulas, optimized/processed product formulas, THRIFT, and the corresponding trick cards.

## Implemented Corrections

- Added missing global-error and locality context to the Trotter-error note: one-step versus \(r\)-step scaling is now separated, and local Hamiltonian commutator cancellations are described through small connected clusters.
- Tightened qDRIFT and randomized-product-formula notes: guarantees are now stated for averaged channels, individual sampled circuits are not promised to be close, \(\lambda=\sum_j |h_j|\) is the relevant qDRIFT scale, and qDRIFT is distinguished from permutation randomization.
- Scoped randomized-order product-formula claims: randomization is no longer described with overbroad fixed-order comparisons, and locality/commutator structure is treated as complementary rather than automatically captured by randomization.
- Corrected lattice-simulation scope: the Childs-Su lattice result is framed for geometrically local lattice Hamiltonians, constant target accuracy is explicit, and practical product-formula wins are limited to the tested regimes.
- Repaired Low-Kliuchnikov-Wiebe multiproduct details: query complexity, coefficient-state preparation, QSP comparison, normalization/condition number language, and over-strong uniqueness wording were corrected.
- Rewrote randomized-MPF material in both the paper note and trick card: observable estimation now uses the required double sum over \((q,q')\), sampled interference terms, \(\Xi^2\) rescaling per sample, and \(\Xi^4\) variance scaling. The old single-index mixture estimator is explicitly marked incorrect for coherent MPF expectations.
- Clarified randomized order-doubling: approximation order, local-error power, expectation-only correction generators, and channel-versus-unitary output are now separated.
- Added convention and scope caveats to commutator-exponential notes: anti-Hermitian/Hermitian conventions are explicit, commutator lower bounds are not compared directly to ordinary Hamiltonian-simulation QSP/LCU bounds, and Chen et al.'s six-gate formula is presented with the actual \(S_3\) coefficients.
- Corrected CPF and THRIFT comparisons: THRIFT is dated as 2024 arXiv / 2025 journal, its primitive is time-ordered evolution of interaction-picture terms or \(A+\alpha H_j\)-type pieces, and the \(\alpha^2\) optimality claim is scoped to the time-ordered-product-formula model.
- Scoped Morales et al. claims: "8th order optimal" and "10th order not useful" are limited to the paper's tested parameter ranges, Hamiltonian classes, and cost model. Personal relevance wording was removed.
- Updated trick cards for resolution factors, processed formulas, eigenvalue-error metrics, recursive commutator formulas, symplectic correctors, and optimal-order selection. In particular, the optimal Suzuki order now scales as \(k^*\approx\sqrt{\log(C_{\mathrm{eff}}\Lambda t/\epsilon)/(2\log 5)}\), matching the standard \(\exp(O(\sqrt{\log}))\) optimized complexity.

## Feedback Not Directly Implemented

- I did not turn randomization caveats into blanket no-go statements. Where the supervisor feedback pointed to a limitation, the notes now distinguish theorem-level restrictions from diagnostic hypotheses.
- I did not remove every occurrence of phrases like "single-index" or "mixture"; in the randomized-MPF targets those phrases are retained only as explicit warnings about the incorrect estimator.
- Some wording was narrowed rather than copied literally, especially for product-formula versus QSP/LCU comparisons, because the correct comparison depends on normalization, access model, and whether block-encoding construction cost is included.

## Verification

- Scoped residual searches for the main Tranche 7 bad strings return no matches on the target files.
- A scoped `git diff --check` pass and direct trailing-whitespace scan are clean on the Tranche 7 targets.
