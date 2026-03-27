
> **Source:** Aaronson & Gottesman, arXiv:quant-ph/0406196 (2004); also a standard quantum circuit identity
> **Tags:** #trick #complexity #measurement #circuit-identities #parityl

## What it does

Moves all measurements to the end of a circuit by replacing mid-circuit measurements + classically-controlled operations with purely quantum (coherent) operations, without changing output statistics. Used to prove that stabilizer circuits are in ⊕L by reducing them to purely unitary CNOT circuits.

## The trick

**Standard deferred measurement:** Any mid-circuit measurement on qubit $q$ followed by a classically-controlled unitary $U$ can be replaced by a coherent controlled-$U$ gate (controlled on $q$, without measuring), followed by measuring $q$ at the end. The joint output distribution is identical.

For stabilizer circuits, all gates are in $\{$CNOT, $H$, $S$, measure$\}$ and all classically-controlled corrections are Pauli corrections. The deferred-measurement version replaces every measurement-controlled Pauli correction with a CNOT (or CNOT + $H$/$S$) gate, yielding a purely unitary stabilizer circuit followed by all measurements at the end.

**Application to ⊕L proof (Aaronson-Gottesman):**
1. Defer all measurements → purely unitary stabilizer circuit + final $Z$-basis measurements.
2. Final $Z$-basis measurements on a stabilizer state yield a parity function of CNOT-network outputs (since CNOT networks compute linear maps over $\mathbb{F}_2$).
3. ⊕L is exactly the class of problems reducible to evaluating a parity of a CNOT-network output — so the simulation is in ⊕L.
4. Hardness follows because CNOT-only circuits are a subclass of stabilizer circuits and are ⊕L-hard.

## When to reach for it

- Proving containment of a circuit class in a complexity class: defer measurements to collapse measurement-based branching into linear algebra.
- Simplifying circuit analysis: a circuit with mid-circuit measurements is often easier to study once measurements are deferred (pure state evolution throughout).
- Fault-tolerant circuit compilation: error-syndrome measurements can sometimes be deferred to reduce critical path depth.

## Complexity

Zero additional gate overhead for the reduction (deferred CNOT is the same gate). Can introduce ancilla qubits if the classically-controlled unitary acts on a different register, but for stabilizer circuits no ancillas are needed.

## Caveat

Deferred measurement increases the number of qubits in the live circuit (measured qubits must stay coherent). For hardware implementations, this trades gate depth for qubit count. In fault-tolerant contexts, keeping qubits live longer increases error accumulation — so the theoretical trick has practical cost.

Also: the principle applies only when the classically-controlled operations are themselves quantum gates. It does not apply when the measurement outcome is used for classical post-processing outside the circuit.

## Related notes
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]]
