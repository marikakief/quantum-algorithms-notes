# Review: Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021)

Source note: [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2002.11649
- PRA DOI linked in the source note.

## Verdict

Minor issues. The note captures why QSPPACK mattered. The main needed corrections are to label the large-degree performance claims as numerical evidence rather than theorem guarantees, and to clean up cross-reference metadata around eigenstate filtering.

## Line-Anchored Comments

- Lines 15-23: Good description of the practical bottleneck. It would be more careful to say prior recursive/factorization methods become numerically difficult in standard precision, rather than presenting the degree cutoffs as hard limits.

- Lines 19-20 and 75-83: The degree `>10,000` and `10^{-12}` claims are numerical demonstrations, not general convergence theorems. The paper's strength is empirical robustness plus a well-designed objective/initialization, not a proof that L-BFGS always succeeds.

- Lines 31-37: The symmetric-phase variable count is handled well. It may help to distinguish the full length `d+1` phase list from the `e_d = ceil((d+1)/2)` independent variables.

- Lines 39-43: The Chebyshev-node loss explanation is strong. Keep the distinction between nodewise loss, Chebyshev coefficient control, and sup-norm control visible; they are not identical norms.

- Lines 47-53: The perturbative initial guess is well explained. "Scaling-and-squaring" should be described as a phase-construction strategy, not as a guarantee that high-norm targets have the same optimization landscape.

- Line 109: The Hamiltonian-simulation polynomial degree should retain the usual `t + log(1/epsilon)/loglog(1/epsilon)` form in the QSP/Jacobi-Anger setting unless the note is intentionally using a coarse bound.

- Lines 144-155: The cross-link to `Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020)` inherits metadata problems flagged elsewhere in the vault. Check the actual authors of that filtering paper before calling it "Dong, An & Lin" or "same group."

## Missing or Suggested Additions

- Add one sentence tying this to the later Wang-Dong-Lin landscape paper: Dong et al. provide the working optimizer; Wang et al. prove local convexity only in a restricted small-norm regime.

- Add a warning that phase-factor computation is classical preprocessing; it does not change the quantum query complexity, but it can dominate practical compilation.

