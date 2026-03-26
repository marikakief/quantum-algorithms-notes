# Gradient Encoding for Multi-Observable Estimation

> **Source:** Huggins, Wan, McClean, O'Brien, Wiebe, Babbush, arXiv:2111.09283
> **Tags:** #trick #measurement #expectation-value #gradient-estimation #fault-tolerant

## What it does

Estimates $M$ expectation values $\langle \psi | O_j | \psi \rangle$ simultaneously using $\tilde{O}(\sqrt{M}/\varepsilon)$ queries to the state preparation oracle, by encoding all $M$ values as the gradient of a single scalar function.

## The trick

Define a parameterised unitary $U(x) = \prod_{j=1}^{M} e^{-2ix_j O_j}$ and the function:

$$f(x) = -\frac{1}{2}\operatorname{Im}\langle \psi | U(x) | \psi \rangle + \frac{1}{2}$$

Then $\partial f / \partial x_\ell |_{x=0} = \langle \psi | O_\ell | \psi \rangle$, so the gradient at the origin gives all $M$ expectation values.

Build a [[Probability Oracle from Hadamard Test|probability oracle]] for $f$ using the Hadamard test (one $U_\psi$ query per oracle call), add quantum controls to the rotation parameters, and apply the Gilyen-Arunachalam-Wiebe quantum gradient estimation algorithm. The gradient algorithm uses higher-order finite differences, phase kickback, and the quantum Fourier transform to extract all $M$ gradient components from $\tilde{O}(\sqrt{M}/\varepsilon)$ oracle queries.

The derivative bound $|\partial_\alpha f(0)| \leq 2^{k-1}$ for $k$-th order partials ensures the analyticity condition needed by the gradient algorithm is satisfied with $c = 2$.

## When to reach for it

- Measuring many properties of a fault-tolerant quantum simulation: RDMs, forces, dipole moments, correlation functions — anything that's a collection of expectation values.
- Specifically when $\varepsilon < 1/(3\sqrt{M})$ (high-precision regime). Outside this regime, sampling-based methods like [[Basis Rotation Grouping for VQE Measurement]] or shadow tomography can be cheaper.
- When the state preparation cost dominates over the cost of implementing controlled time evolution by the observables. If the observables are expensive to exponentiate, the advantage narrows.
- For $k$-RDMs of $N$-mode systems: gives $\tilde{O}(N^k/\varepsilon)$ queries, beating both sampling ($\tilde{O}(N^k/\varepsilon^2)$) and amplitude estimation ($\tilde{O}(N^{2k}/\varepsilon)$).

## Complexity

- **State preparation queries:** $\tilde{O}(\sqrt{M}/\varepsilon)$
- **Controlled time evolution gates per observable:** $\tilde{O}(\sqrt{M}/\varepsilon)$, each with $|x| \in O(1/\sqrt{M})$
- **Total time evolution (all observables):** $\tilde{O}(M/\varepsilon)$
- **Ancilla qubits:** $O(M\log(1/\varepsilon))$ for the index registers, beyond the $N$-qubit system

## Caveat

The $\sqrt{M}$ advantage is only in state preparation queries. Total controlled time evolution is still $\tilde{O}(M/\varepsilon)$, same as amplitude estimation applied to each observable separately. The ancilla overhead is $O(M\log(1/\varepsilon))$ qubits, which can dominate the space cost when $M \gg N$. A space-query tradeoff exists: partition into $g$ groups, using $\tilde{O}(N + M/g)$ qubits at $\tilde{O}(\sqrt{gM}/\varepsilon)$ query cost.

No practical fault-tolerant resource estimates exist yet — the constant factors in the $\tilde{O}$ could be large enough to shift the crossover with sampling-based methods.

## Related notes
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]]
- [[Probability Oracle from Hadamard Test]]
- [[Non-Uniform Gradient Estimation via Variable-Precision QFT]]
- [[Pauli Expectation Value Estimation]]
- [[Basis Rotation Grouping for VQE Measurement]]
- [[Analytical Gradient Estimation for Variational Quantum Circuits]]
