# Tranche 6 Resolution

## Scope

Processed the Tranche 6 feedback covering the Hamiltonian-simulation spine: Lloyd product formulas, sparse-Hamiltonian simulation, black-box walks, LCU/OAA, Taylor-LCU simulation, Bessel-LCU, QSP, qubitization, QSVT, and the corresponding trick cards.

## Implemented Corrections

- Qualified Lloyd 1996 throughout: finite-dimensional local/bounded-strength systems, physical "real time" versus circuit depth, bounded-degree parallelization assumptions, and entropy pumping versus arbitrary state preparation.
- Corrected sparse-Hamiltonian notes: edge-coloring produces matchings, not perfect matchings; lower bounds are scoped to the sparse black-box model; the historical comparison table now distinguishes BACS, Childs-Wiebe multi-product/LCU, Taylor-LCU, and QSP/qubitization correctly.
- Tightened Berry-Childs 2011: row/column norm conventions, complex square-root phases, laziness versus simulation error, required norm promises, and the status of the \(N^{2/3}\) state-preparation cost.
- Fixed Childs-Wiebe 2012: \(\Delta\) is an operator-norm error/failure bound, negative coefficients need phase handling, and the "better than product formulas" comparison is limited to the older single-product-formula setting.
- Corrected BCCKS 2014 and Taylor 2015: removed the false \(1/N^N\approx1/N!\) claim, identified the \(d^2\) sparse-decomposition source, and replaced "QSP removed the loglog factor" with the correct additive-vs-multiplicative distinction.
- Updated BCK 2015 and Low-Chuang QSP/qubitization notes so the main comparison is
  \[
  \text{Taylor/Bessel-LCU: } O(T\log(T/\epsilon)/\log\log(T/\epsilon)),\qquad
  \text{QSP/qubitization: } O(T+\log(1/\epsilon)/\log\log(1/\epsilon)).
  \]
- Repaired standard-form and qubitization normalization: notes now display \(H/\alpha\), use eigenangle \(\arccos(\lambda/\alpha)\), and state simulation complexity as \(O(\alpha t+\log(1/\epsilon)/\log\log(1/\epsilon))\).
- Rewrote the truncated Taylor trick card around segmented \(T=\lambda t\), \(K=\Theta(\log(T/\epsilon)/\log\log(T/\epsilon))\), nonnegative PREPARE weights, SELECT phases, \(s\approx2\), robust OAA, and \(O(K\log L)\) ancillas.
- Corrected LCU/OAA trick cards: coefficients use \(\sum_j|\beta_j|\), success probability is \(\|\tilde U|\psi\rangle\|^2/s^2\), OAA requires a uniform success amplitude and a unitary or near-unitary target, and robust OAA is not a generic postselection remover.
- Tightened product-formula trick cards: first-order error uses commutator sums, full \(r\)-step error is separated from one-step error, optimized Suzuki bounds are scoped to Suzuki recursion, and order-condition cancellation is aggregate cancellation through order \(p\), not individual monomial disappearance.
- Updated spectral-amplification and qubitization trick cards to distinguish spectroscopy via phase estimation from real-time simulation via QSP, and to avoid claiming unconditional worst-case improvements over QSP.
- Added QSVT admissibility/checklist content: normalization first, parity/boundedness/degree constraints, and explicit accounting for block-encoding cost and normalization.

## Feedback Not Directly Implemented

- Some suggested replacements were not copied literally because the literal phrasing would still be misleading. In particular, QSP did not simply "remove a loglog factor"; it made the time and precision dependence additive while retaining the \(\log/\log\log\) precision term.
- The uniform spectral amplification low-energy comparison was reframed away from \(O(\alpha\sqrt{\Delta})\) versus \(O(\alpha\Delta)\), since that comparison is inverted for \(\Delta<1\). The corrected note compares against full-normalization simulation and states the low-energy promise.
- Residual similar phrases in unrelated notes outside Tranche 6 were left untouched; this pass only applied the supervisor feedback indexed under Tranche 6.

## Verification

- Targeted residual searches found no remaining Tranche 6 instances of the main bad strings: `removed the loglog`, `log\\log factor`, `any desired state` in the Lloyd note, `essentially all modern quantum algorithms`, old `H = (<G|...)` standard-form normalization, and raw `O(t + log...)` qubitization complexity. The only remaining `perfect matching` occurrence in the sparse-simulation targets is the corrected phrase "not necessarily a perfect matching."
- A follow-up diff/stat pass covered all 29 Tranche 6 target files.
