# Tranche 12 Resolution

Scope: arithmetic/QFT/adder primitive notes reviewed in Tranche 12.

## Paper notes

- `A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025)`: implemented the review suggestions. The note now states that Z-redundant carries are computational-basis functions of surviving register values, that the dirty target register must be restored rather than consumed, and that the "free control" claim is scoped to the paper's classical-offset structure. The depth discussion now says the construction is linear-depth and that known lower-depth approaches spend workspace or use different primitives, rather than asserting a general constant-workspace lower bound. Added a comparison to the 2025 log-depth optimistic QFT work as a different depth/workspace/error tradeoff.

- `A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025)`: implemented the major corrections. The note now consistently says the optimistic QFT has logarithmic gate range/locality in a 1D layout, not nearest-neighbor locality. The inter-block phase denominator uses the paper's two-block convention, with `2^{2m}` for the direct phase rotation. The Shor/factoring discussion is framed through the paper's distributional argument for intermediate modular-exponentiation values, not a universal worst-case replacement claim. Added an explicit table separating optimistic Frobenius-average error, randomized worst-input expected error over the 1-design sample, and deterministic coherent error after purification.

- `Addition on a Quantum Computer (Draper 2000)`: implemented the convention and resource-accounting fixes. The note now uses the paper's `|a>|b> -> |a+b>|b>` convention consistently, marks the `3n` reversible-adder comparison as historical, distinguishes abstract rotations from fault-tolerant synthesis cost, scopes the `O(log n)` parallel-depth claim to the addition rotations only, and qualifies the Shor qubit-count comparison as the historical Zalka/Draper setting.

- `A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004)`: implemented the comparison fixes. The QFT-adder comparison now says full QFT-adder depth is implementation/connectivity/ancilla dependent. The T-count discussion no longer equates linear Toffoli count directly with linear T count; it notes the dependence on Toffoli decomposition and compute/uncompute pairing. Added the modern contrast that QCLA spends workspace for depth, whereas Gidney-style ripple/CQ adders optimize T count and workspace at linear depth.

## Trick cards

- `Arithmetic in the Fourier Basis`: implemented. Replaced the misleading "gate depth" phrasing with abstract rotation count, added a complexity table separating addition rotations from the full QFT-add-inverse-QFT adder, and added a fault-tolerant synthesis caveat.

- `Quantum Fourier Transform Circuit`: mostly already satisfied from prior correction. Kept the corrected `H_j`/`CP_m` notation, target-error AQFT cutoff, connectivity-qualified depth table, and output-bit-reversal caveat. Added that the 2025 optimistic QFT has logarithmic-range 1D locality.

- `Optimistic Quantum Circuits`: implemented. Replaced the nearest-neighbor claim with logarithmic gate range/locality, qualified the Shor drop-in discussion as relying on the paper's random-`r` modular-exponentiation argument, and added a warning that Frobenius-average error is not a promise about arbitrary user-chosen input distributions.

- `Quantum Phase Estimation on Blocks for QFT Parallelisation`: implemented. The inter-block phase now uses `exp(2 pi i X_{i+1}Y_i/2^{2m})`, the displayed block state is normalized, and the Lee metric/wraparound convention is explicit.

- `Worst-to-Average-Case Reduction for Optimistic Circuits`: implemented. Replaced the informal "effectively random" phrasing with the 1-design expected-squared-error argument, and clarified that purification gives a deterministic coherent circuit on an enlarged register, not the same circuit on the original register alone.

- `Carry Venting via X-Basis Measurement`: implemented. Added the modulo/carry-in convention for the complement-carry identity, scoped the carry-xor cost to internal carry phasing tasks with boundary dependence on variant, and added the compute-copy-uncompute contrast.

- `Half-Register Dirty Workspace Borrowing`: implemented. Added the requested phase table showing when each half is active data, dirty workspace, or finished data.

- `Carry-Lookahead Tree for Logarithmic-Depth Addition`: implemented. Added the parallel-prefix vs. MAJ/UMA ripple distinction and qualified the Fourier-basis related note so it does not imply a model-independent full-adder `O(log n)` depth.

- `Phase Overlapping in Tree Circuits`: implemented. Added a level-index table showing which `P_t`, `G_t`, `C_t`, and `P^{-1}_t` data are used and which phases can overlap.

- `MAJ-UMA Decomposition for Reversible Adders`: no edit needed. The existing note already contains the requested wire-order warning, source-variant caveat, boundary-variant caveat, and paired-Toffoli explanation.

- `Two's-Complement Carry Erasure for In-Place Adders`: implemented. Added a bit-level full-adder derivation showing that the same carry-in `c_i` in the `a + overline{s}` erasure addition produces output bit `overline{b_i}` and carry-out `c_{i+1}`.
