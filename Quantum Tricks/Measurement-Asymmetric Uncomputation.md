# Measurement-Asymmetric Uncomputation

> **Source:** Gidney, arXiv:1709.06648; principle also in Berry, Gidney et al. arXiv:1902.02134
> **Tags:** #trick #circuit-primitive #measurement #uncomputation #fault-tolerant #surface-code

## What it does

Makes uncomputation cheaper than computation by replacing the reverse circuit with a measurement and classically conditioned Clifford correction. In the [[Temporary Logical-AND]], this reduces the erase cost from 4 T gates to zero.

## The trick

When a qubit $|a\rangle$ has been computed as a deterministic function of other qubits (i.e., the computation is a classical reversible function applied coherently), erasing $|a\rangle$ does not require running the computation in reverse. Instead:

1. **Measure** $|a\rangle$ in the X basis (apply $H$, then measure in the computational basis).
2. **Classically condition** a Pauli $Z$ correction on the qubits that $|a\rangle$ was entangled with, based on the measurement outcome.

This works because the X-basis measurement projects the ancilla onto $|+\rangle$ or $|-\rangle$, collapsing it to a product state with the rest of the system. The $Z$ correction fixes any residual phase.

The key insight: computation requires non-Clifford gates (T gates) to create entanglement between the ancilla and the controls. But *disentangling* can be done by measurement alone, which is a Clifford-class operation in the surface code. Measurement is irreversible, which is why it can do something that reversible unitary operations cannot.

This principle appears repeatedly in fault-tolerant circuit design:

- [[Temporary Logical-AND]]: compute AND for 4 T, erase for 0 T
- [[Measurement-Based QROM Uncomputation]]: erase a QROM output register at $O(\sqrt{d})$ Toffoli cost, independent of output width
- [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] uncomputation: same principle applied to space-time traded lookups
- Out-of-place adders: the inverse of Gidney's out-of-place adder uses 0 T gates

## When to reach for it

- Any reversible computation followed by its inverse (compute-use-uncompute pattern), where the uncomputation can be replaced by measurement.
- Particularly valuable when the forward computation is T-heavy and the uncomputation would otherwise duplicate the T cost.
- Most natural in the surface code, where measurement is a primitive operation with no additional overhead and Pauli corrections are tracked in software (frame tracking).

## Complexity

The uncomputation cost drops from matching the forward computation to a single layer of measurements plus classical processing. In the surface code, the measurement layer has the same latency as a single surface code cycle.

## Caveat

Requires mid-circuit measurement and real-time classical feedforward. In the surface code this is free (measurement *is* the basic operation, and Pauli frame tracking handles corrections in software). In other architectures (ion traps, photonic), measurement may introduce latency or require different circuit structures.

The classical correction must be computed and applied before the next non-Clifford operation on the affected qubits. For circuits with many simultaneous measurements, the classical processing can become a bottleneck (though current estimates suggest this is not a practical concern for near-term fault-tolerant devices).

## Related notes
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — source paper, introduces the principle via the temporary logical-AND
- [[Temporary Logical-AND]] — primary application
- [[Measurement-Based QROM Uncomputation]] — same principle for data lookup
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — applies the principle throughout
- [[Sum of Tree Sums for Bit Counting]] — uses measurement-based uncomputation for tree sum erasure
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — extends the principle to "venting" carry qubits; see [[Carry Venting via X-Basis Measurement]]
