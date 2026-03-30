# Measurement-Driven Computation via Stabiliser Relations

> **Source:** Raussendorf & Briegel, arXiv:quant-ph/0010033
> **Tags:** #trick #MBQC #stabiliser #measurement #computation

## What it does
Converts single-qubit measurements on an entangled state into deterministic logical operations on unmeasured qubits, using stabiliser eigenvalue equations as the mechanism.

## The trick
A cluster state satisfies $\sigma_x^{(a)} \prod_{a' \in \mathrm{ngbh}(a)} \sigma_z^{(a')} |\Phi\rangle = \pm |\Phi\rangle$ for every site $a$. When you measure qubit $a$ in a chosen basis, the stabiliser relations force the state of the remaining qubits into a specific form. The key insight: measuring a qubit in the $\sigma_z$ basis effectively removes it from the cluster (disconnects it from its neighbours). Measuring in the $\sigma_x$ basis propagates quantum information along a chain. Measuring in the $x$-$y$ plane at angle $\phi$ implements a rotation $U_z(\phi)$ up to byproducts.

For a wire (chain of qubits), this means:
- $\sigma_x$ measurements = identity (teleportation)
- Measurements at angle $\phi$ in the $x$-$y$ plane = rotation by $\phi$ around the $z$-axis

For a CNOT, a vertical qubit attached to a horizontal wire acts as a conditional switch: its $\sigma_z$ eigenvalue controls whether the wire implements the identity or $\sigma_x$.

## When to reach for it
- Any setting where you have a stabiliser state and want to "program" it via measurements
- Designing protocols where the computation is encoded in measurement choices rather than unitary operations
- Blind quantum computation: the server prepares the resource, the client specifies only measurement angles

## Complexity
Each gate consumes a constant number of cluster qubits. The total cluster size is $O(nT)$ for an $n$-qubit circuit with $T$ gates.

## Caveat
Only works for states with the right stabiliser structure. A random entangled state won't do — the stabiliser equations are what make measurements deterministic (up to known byproducts). The cluster state is special in this regard.

## Related notes
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]]
- [[Cluster State as Universal Entanglement Resource]]
- [[Byproduct Propagation in MBQC]]
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
