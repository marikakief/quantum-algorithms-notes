# Second-to-First Quantization State Conversion

> **Source:** Babbush, Huggins, Berry et al., arXiv:2301.01203 (2023)
> **Tags:** #trick #first-quantized #state-preparation #Slater-determinant #Givens-rotations

## What it does

Converts a Slater determinant state from second-quantized representation (occupation-number encoding, $N$ qubits) to first-quantized representation ($\eta$ registers of $\log N$ qubits) on-the-fly during preparation. Uses only $O(\eta)$ ancilla qubits rather than $N$ qubits for the intermediate second-quantized state.

## The trick

Preparing a Slater determinant in first quantization directly is awkward — the antisymmetrisation and orbital coefficient structure are naturally expressed in second quantization via [[Givens Rotation Slater Determinant Preparation|Givens rotations]]. But storing the full second-quantized state requires $N$ qubits, defeating the space advantage of first quantization ($O(\eta \log N)$ qubits).

The trick: the [[Givens Rotation Slater Determinant Preparation|Givens rotation]] network on $N$ qubits produces the qubits *sequentially* — qubit $N$ is finished first, then $N-1$, and so on. Once a qubit is done (no more rotations act on it), it's either $|0\rangle$ (unoccupied) or $|1\rangle$ (occupied, possibly entangled with earlier qubits via the Givens cascade).

**Conversion protocol:**
1. Maintain only $O(\eta)$ "active" second-quantized qubits at any time
2. After the Givens rotation network finishes with a qubit:
   - If $|0\rangle$: discard it (nothing to record)
   - If $|1\rangle$ (occupied): write the orbital index into the next available first-quantized particle register ($\log N$ qubits), controlled on the qubit being $|1\rangle$
   - Reset the second-quantized qubit for reuse
3. After all $N$ qubits have been processed, the first-quantized registers hold the correct antisymmetric state

Since at most $\eta$ qubits are occupied at any time, only $O(\eta)$ second-quantized qubits need to be "live" simultaneously. Each of the $O(N)$ conversion steps touches $O(\eta \log N)$ qubits (to update the particle registers), giving a total gate cost of $O(N \eta \log N)$.

The Toffoli complexity can be further reduced to $\widetilde{O}(N\eta)$ by batching the index writes.

## When to reach for it

- Initialising first-quantized dynamics simulations from mean-field (Slater determinant) states
- Any first-quantized algorithm that needs a Slater determinant initial state without paying $O(N)$ space overhead
- Finite-temperature simulations where you sample different Slater determinants from a thermal distribution — each sample needs a fresh state preparation

## Complexity

- **Space:** $O(\eta \log N + \eta)$ qubits total ($\eta$ particle registers + $O(\eta)$ ancillae)
- **Toffolis:** $\widetilde{O}(N\eta)$
- **This is a one-time cost,** not multiplied by the simulation duration. For long time evolution, it's subdominant.

## Caveat

- Still costs $O(N\eta)$ — for very large $N$ this may not be negligible compared to short dynamics simulations
- Requires knowing the Givens rotation angles classically, i.e. classically diagonalising the $N \times N$ one-particle density matrix (which is $O(N^3)$ classically but a one-time cost)
- The conversion introduces some complexity in bookkeeping the "done" qubits; implementation details matter for constant factors

## Related notes
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]]
- [[First-Quantized Plane-Wave Chemistry Encoding]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
