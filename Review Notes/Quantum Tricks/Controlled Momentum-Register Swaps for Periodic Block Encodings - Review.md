# Review: Controlled Momentum-Register Swaps for Periodic Block Encodings

Source note: [[Controlled Momentum-Register Swaps for Periodic Block Encodings]]

## Primary Sources Checked

- Rubin et al., arXiv:2302.05531 / PRX Quantum 4, 040303 (2023).

## Verdict

Minor issues. The implementation idea is clear, but the lower-bound language is broader than the argument supports.

## Line-Anchored Comments

- Line 20: "one Toffoli per controlled SWAP" is fine for the usual decomposition, but it should be labeled as a decomposition-level estimate including Clifford gates for free.
- Line 24: "must physically touch every qubit at least once" is true for this explicit register-swap implementation. It is not a lower bound for every conceivable encoding or architecture.
- Line 37: `Omega(N_k N)` for any second-quantized periodic block encoding is too broad. Say it is a lower-bound intuition for block encodings that implement the needed basis relabeling by touching explicit momentum/orbital registers.

## Missing or Suggested Additions

- Add a sentence distinguishing algorithmic oracle cost from routing/connectivity overhead. The card currently treats register touches as if they immediately imply architecture-independent Toffoli cost.

