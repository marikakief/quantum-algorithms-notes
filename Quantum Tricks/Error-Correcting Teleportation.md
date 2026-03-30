# Error-Correcting Teleportation

> **Source:** Knill, arXiv:quant-ph/0410199; builds on Gottesman-Chuang (1999)
> **Tags:** #trick #fault-tolerance #teleportation #error-correction #syndrome

## What it does
Combines error correction with quantum teleportation in a single step: the logical state is teleported to a fresh block while simultaneously revealing syndrome information for error correction. This minimises the number of noisy gates between error corrections.

## The trick
Standard error correction has (at least) two steps: (1) extract the syndrome by coupling to ancilla qubits, (2) apply a correction based on the syndrome. Each step involves noisy gates that can introduce new errors before the old ones are corrected.

Error-correcting teleportation does it all at once:

1. **Prepare a logical Bell pair**: two blocks, each encoding a logical qubit pair, in a maximally entangled state. The preparation can be done offline with verification.

2. **Transversal Bell measurement**: apply CNOT from each physical qubit in the computational block to the corresponding qubit in the first block of the Bell pair, then measure both.

3. **State transfer**: by the teleportation protocol, the logical state now resides in the second block of the Bell pair, up to a Pauli correction determined by the measurement outcomes.

4. **Error information**: the Bell measurement outcomes simultaneously reveal the syndrome of the *combined* errors from the computational block and the first Bell-pair block. If the combined error is within the code's correction capacity, it can be determined and the Pauli frame updated.

The result: error correction happens in a single transversal step (one round of CNOTs + measurements), not two or more sequential steps. The fresh second block carries the logical state with errors corrected and no accumulated damage from multi-round syndrome extraction.

## When to reach for it
- In any concatenated fault-tolerance scheme where syndrome extraction is a bottleneck
- When you want to simultaneously perform a logical gate and correct errors (gate teleportation + error correction)
- When leakage errors are a concern — teleportation automatically resets the physical qubits to the code space, controlling leakage at each step
- When you can afford the cost of preparing verified logical Bell pairs offline

## Complexity
Requires preparing one logical Bell pair per error-correction step. The Bell pair preparation dominates the resource cost, especially at high error rates where verification (to ensure approximately independent errors between the two blocks) requires purification or entanglement swapping. The teleportation step itself is just transversal CNOTs + measurements — depth 2.

## Caveat
- Requires more complex state preparation than Steane-style syndrome extraction (logical Bell pairs vs. ancilla blocks)
- The quality of the prepared Bell pair directly limits the error-correction capability. If the Bell pair has correlated errors between its two blocks, the correction can fail.
- Pauli frame tracking requires classical computation to interpret measurement outcomes. For non-Clifford gates (where the compensation may not be a Pauli), you must wait for measurement results before proceeding.

## Related notes
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]]
- [[Error-Detecting Codes with Concatenation for Fault Tolerance]]
- [[Postselected Computation for State Preparation]]
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
