# Review: Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009)

Source note: [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]]

## Verdict

Minor-to-major issues. The note captures the conceptual importance of dissipative computation, but it overstates the self-correction/practical fault-tolerance message and slightly blurs preparation of a ground space with preparation of an arbitrary desired ground state.

## Comments

- "Self-correcting" is too strong if read in the fault-tolerance sense. The dissipative dynamics are attractive toward a steady state under the ideal Lindbladian, but this is not a threshold theorem against arbitrary implementation noise.

- The DQC construction encodes the output in a clock/history mixture and succeeds on the final clock time with probability `1/(T+1)`. The note states this; keep it adjacent to any universality or runtime slogan.

- The dissipative state-engineering claim should distinguish frustration-free ground spaces from a specified pure ground state. In degenerate cases the dissipative map may prepare or stabilize a subspace rather than select a unique vector.

- The Lindblad-operator formulas should be checked against the paper before editing the original note. Small sign/direction differences in jump operators change the steady state, even if the high-level history-state idea is right.

- The graph-state gap equal to 1 is a special construction, not representative of general frustration-free Hamiltonians.

## Suggested Additions

- Add a terminology note: attractive steady state, passive correction, and fault-tolerant self-correction are different claims.
- Add a small table separating DQC convergence, readout overhead, DSE convergence, and implementation engineering assumptions.
