
> **Source:** Bremner, Jozsa, Shepherd, Proc. R. Soc. A 467 (2011); arXiv:1005.1407
> **Tags:** #trick #IQP #post-selection #circuit-compilation

## What it does
Replaces an intermediate Hadamard gate in a quantum circuit with a CZ gate, an ancilla, and a post-selection — converting any universal quantum circuit into an IQP (commuting-gates-only) circuit at the cost of post-selections.

## The trick

Given a state $|\psi\rangle_a$ entering a Hadamard gate $H_a$ in the middle of a circuit:

1. Introduce an ancilla $|0\rangle_e$
2. Apply $H_a \cdot \text{CZ}_{ae} \cdot H_e$ (all are valid IQP gates or boundary Hadamards)
3. Post-select qubit $a$ on outcome $|0\rangle$
4. The resulting state on line $e$ is exactly $H|\psi\rangle$

Line $e$ now carries the output of the Hadamard and continues through the rest of the circuit.

Apply this to every intermediate Hadamard gate → the circuit becomes an IQP circuit (all gates diagonal in $X$ or $Z$ basis, Hadamards only at boundaries) with additional post-selections.

## When to reach for it
- Proving that post-$\mathcal{C}$ = PP for a restricted circuit class $\mathcal{C}$ — the Hadamard gadget is the standard tool for showing that post-selection can compensate for lack of non-commuting gates
- Compiling circuits into IQP form for quantum supremacy arguments
- Any situation where you need to remove intermediate basis changes at the cost of post-selection

## Complexity
Each intermediate Hadamard costs one ancilla qubit and one post-selection. A circuit with $g$ intermediate Hadamards becomes an IQP circuit with $g$ extra qubits and $g$ post-selections.

## Caveat
Post-selection is not physically free — it incurs an exponential overhead in practice (you discard all runs that don't satisfy the post-selection condition). This gadget is a theoretical tool for complexity classification, not a practical circuit optimisation.

## Related notes
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]]
- [[Post-Selection Boosting for Hardness of Sampling]]
