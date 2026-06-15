# Review: Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002)

Source note: [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]]

## Verdict

Minor-to-major issues. The core recursive-bisection account is right, but the note should be much stricter about what "efficiently integrable" means. The algorithm is not a free state-preparation primitive for arbitrary samplable or numerically integrated distributions.

## Comments

- The key condition is deterministic, coherent, sufficiently precise evaluation of interval weights or CDF differences. If those quantities are only available by Monte Carlo sampling, the sampling cost can erase the quantum advantage.

- The discussion of probabilistic classical integration is risky. Coherently embedding random estimators and uncomputing them does not automatically produce the exact rotation angles needed for a high-fidelity state. The approximation error has to be budgeted across all conditional rotations.

- The note should keep arithmetic precision visible. Each split ratio must be computed and converted to a controlled rotation with enough bits that the accumulated state-preparation error is below the target tolerance.

- Grover-Rudolph prepares amplitudes proportional to square roots of a discretized probability distribution. It does not by itself solve the loading problem for arbitrary signed amplitudes or complex phases.

- The later Herbert caveat belongs near the headline, not only in downstream finance notes: for quantum Monte Carlo, state preparation must be cheaper than the mean-estimation advantage it is supposed to enable.

## Suggested Additions

- Add a small error budget: discretization error, interval-integral error, fixed-point division/square-root error, and rotation-synthesis error.
- State examples where the premise is plausible, such as distributions with analytic CDFs or efficiently computable exact interval integrals.
