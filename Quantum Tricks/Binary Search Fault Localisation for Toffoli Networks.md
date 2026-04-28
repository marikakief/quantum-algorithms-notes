# Binary Search Fault Localisation for Toffoli Networks

> **Source:** Häner, Roetteler, Svore, arXiv:1611.07995
> **Tags:** #trick #Toffoli #testing #debugging #fault-tolerant #reversible-computation

## What it does

Locates faulty gates in a Toffoli-only quantum circuit by running it on classical test vectors and applying binary search on the circuit structure. Works because Toffoli networks map computational basis states to computational basis states — no superposition involved in intermediate states.

## The trick

A Toffoli network (circuit composed entirely of NOT, CNOT, and Toffoli gates) acts as a classical reversible permutation on computational basis states. This means:

1. **Classical simulation is efficient.** For any input $|x\rangle$, the correct output can be computed classically by propagating the bit-string through the gate sequence. This takes $O(G)$ time for a $G$-gate circuit.

2. **Output distribution is a delta function.** On correct hardware, measuring the output of a Toffoli network on input $|x\rangle$ always gives the same deterministic result. Any deviation indicates a fault.

3. **Binary search for faults.** If a test vector detects an error:
   - Split the circuit into two halves at the midpoint.
   - Compute the ideal intermediate state classically.
   - Prepare the ideal state, run the second half, and check the output. Also prepare the input, run the first half, and check against the ideal midpoint.
   - Recurse into whichever half shows the error.
   - After $O(\log G)$ rounds, the fault is localised to a single gate.

The initial test set must be chosen to "trigger" all potential faults (analogous to classical VLSI test vectors for stuck-at faults).

## When to reach for it

- Testing fault-tolerant implementations of arithmetic circuits (adders, multipliers, modular exponentiation) that are built entirely from Toffoli gates.
- Debugging early quantum hardware running classically-verifiable circuits.
- As an argument for preferring Toffoli-based circuit designs over QFT-based alternatives: Toffoli networks are classically testable, while QFT-based circuits produce intermediate superpositions that cannot be efficiently checked.

## Complexity

- **Test rounds:** $O(\log G)$ per fault, where $G$ is the total gate count.
- **Classical overhead:** $O(G)$ per round for computing ideal intermediate states.
- **Quantum overhead:** Each round runs a sub-circuit on hardware and measures. The number of shots per round depends on the noise model and desired confidence.

## Caveat

This only works for circuits composed *entirely* of Toffoli/CNOT/NOT gates. Any rotation gate (even the $R_k$ gates in the semi-classical QFT at the end of [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]) produces superposition states that cannot be checked this way.

The method assumes faults are sparse and persistent (the same fault appears on repeated runs). Transient faults or correlated multi-gate failures may require more test vectors and more rounds.

Also, the fault model is limited to "missing-gate" and bit-flip faults. Coherent errors (small rotations) may not produce detectable basis-state deviations on all test vectors.

## Related notes
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — source paper
- [[Dirty Ancilla Constant Addition]] — the Toffoli-only adder that motivates the testability argument
- [[Reversible Modular Exponentiation with Garbage Cleanup]] — the modular exponentiation circuit being tested
