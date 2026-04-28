# Temporary Logical-AND

> **Source:** Gidney, arXiv:1709.06648; equivalent to Jones (2013) ancilla-and-fixup
> **Tags:** #trick #circuit-primitive #T-complexity #Toffoli #fault-tolerant #surface-code

## What it does

Computes the logical-AND of two qubits into an ancilla for 4 T gates, then erases the ancilla for 0 T gates. Replaces any compute/uncompute Toffoli pair (normally 8 T gates) with a 4 T gate operation.

## The trick

**Compute** $|x\rangle|y\rangle|0\rangle \to |x\rangle|y\rangle|x \wedge y\rangle$ (T-count 4, measurement-depth 1):

Consume a $|T\rangle$ state as the ancilla input. Apply $T^\dagger$ to both controls, CNOT from each control into the ancilla, apply $T$ to the ancilla, then $H$ and $S$ to produce the AND result. The four T gates come from the two $T^\dagger$ gates, the $T$ gate, and the $|T\rangle$ state input.

**Erase** $|x\rangle|y\rangle|x \wedge y\rangle \to |x\rangle|y\rangle$ (T-count 0, measurement-depth 1):

Apply $H$ to the ancilla and measure in the computational basis. If the outcome is 1, apply $Z$ to one of the controls. This uses only Clifford gates and measurement.

The asymmetry between compute and erase comes from measurement being irreversible. The erasure exploits the fact that we don't need to preserve the ancilla — we just need to disentangle it from the controls without introducing phase errors.

**Condition for correctness:** Between compute and erase, intermediate operations must not be sensitive to a phase error $(-1)^{xy}$ on the $|x\rangle|y\rangle$ state. This is automatically satisfied when the AND appears in a compute/uncompute pair where intermediate operations act only on the ancilla (the standard use case), but must be checked in more complex circuits.

## When to reach for it

- Any circuit where Toffoli gates come in compute/uncompute pairs: quantum adders, [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] oracles from classical reversible circuits, multi-controlled gates, arithmetic subroutines.
- Multi-controlled NOT gates: iteratively AND pairs of controls to reduce $n$ controls to one representative ancilla, costing $4(n-1)$ T gates total.
- Any surface code computation where T gates are the runtime bottleneck — the temporary logical-AND halves the T-count of every paired Toffoli.

## Complexity

| Operation | T-count | Measurement-depth | Ancilla |
|---|---|---|---|
| Compute AND | 4 | 1 | 1 (from $\lvert T\rangle$ state) |
| Erase AND | 0 | 1 | 0 (ancilla measured away) |
| Paired Toffoli (old) | 8 | 2 | 0 |
| **Savings per pair** | **4 T gates** | 0 | +1 temporary ancilla |

## Caveat

The ancilla is held between compute and erase. In the surface code, this qubit occupies physical space that could host T factories. For circuits with many temporary logical-ANDs held simultaneously over long measurement depths (e.g., ripple-carry adders with $n \gg 1000$), the [[Ancilla Opportunity Cost Analysis|opportunity cost]] of the ancillae can exceed the T-gate savings. See the analysis in [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|the source paper]].

The construction is equivalent to the ancilla-and-fixup method of Jones (2013). Gidney's contribution is the reframing as a general-purpose primitive rather than a single-gate optimisation.

## Related notes
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — source paper
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — the base adder whose paired Toffolis become temporary logical-ANDs
- [[MAJ-UMA Decomposition for Reversible Adders]] — the paired gate structure that creates the compute/uncompute Toffoli pairs
- [[Measurement-Asymmetric Uncomputation]] — the underlying principle
- [[Ancilla Opportunity Cost Analysis]] — when the ancilla cost outweighs the T-gate savings
- [[Unary Iteration]] — uses temporary logical-ANDs for the binary-to-unary sweep
- [[Depth-Optimised Grover Oracle via Parallel Fan-Out]] — applies the technique to Grover oracles
- [[Measurement-Based QROM Uncomputation]] — same measure-and-fixup principle applied to data lookup
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — first major application in a chemistry algorithm
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — Gidney's follow-up extending measurement-based techniques to CQ addition via [[Carry Venting via X-Basis Measurement]]
