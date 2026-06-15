# Review: Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994)

Source note: [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]

## Primary Sources Checked

- Shor, "Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer", FOCS 1994 / SIAM J. Comput. 26, 1484-1509 (1997), arXiv: https://arxiv.org/abs/quant-ph/9508027
- Full arXiv PDF checked: https://arxiv.org/pdf/quant-ph/9508027

## Verdict

Major issues. The conceptual account is strong, but the resource accounting mixes notation and per-run versus repeated-trial costs.

## Line-Anchored Comments

- Lines 9-17: `N` versus `n`

  The note uses `N` for the integer and `n` for bit length, but then quotes asymptotics that in Shor's paper are written in terms of the integer `n`. Make the convention explicit at the top: "Let `m = ceil(log_2 N)` be the input length." Then rewrite all complexities in `m`.

- Line 17: "Factoring an n-bit integer takes O(n^2 log n log log n) gates"

  This is missing a parameter-convention caveat and possibly a log factor depending on whether it is per quantum experiment or the full high-success algorithm. Shor's expanded paper states the quantum factoring algorithm takes `O((log N)^2 (log log N)(log log log N))` steps, where `N` is the integer to be factored. If this note uses `n = log N`, that is `O(n^2 log n log log n)`. But lines 85-87 separately discuss `O(log log N)` repetitions, so the note currently risks double-counting or undercounting. Clarify per trial, expected trials, and Shor's stated final asymptotic.

- Lines 83-87: success probability and repetitions

  Good source match: Shor uses `phi(r)/r > delta/log log r` and `O(log log r)` repetitions for high probability, then notes practical post-processing can reduce expected trials. Tie this directly to the total complexity table.

- Lines 135 and 149: modular exponentiation and total factoring

  The table says total factoring is the same as fast modular exponentiation while also listing `O(log log n)` repetitions. This is inconsistent unless the table row means "per successful expected run under Shor's stated accounting." Rewrite table with rows: "one modular exponentiation", "one order-finding experiment", "high-probability repetition overhead", and "Shor stated asymptotic".

- Lines 137 and 175: "Shor's error-detection trick" / "quantum watchdog"

  This needs a citation to the exact section or should be toned down. The modular-exponentiation cleanup is standard reversible uncomputation. Calling it an early mid-circuit error-detection mechanism is interpretive and could mislead readers into thinking Shor introduced fault-tolerant syndrome-style checking here.

- Lines 165 and 173: Simon to Shor / cyclic Fourier sampling

  Good pedagogically, but add the important approximation: Shor samples a register of size `q` with `N^2 <= q < 2N^2`; the period need not divide `q`, and continued fractions handle the approximation.

## Missing or Suggested Additions

- Add an explicit "Notation" block: integer to factor `N`; bit length `n = log N`; Fourier modulus `q`; order `r`.
- Add a "Per trial vs success amplification" block.
