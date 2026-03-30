# Decoupling by Random Measurement

> **Source:** Horodecki, Oppenheim & Winter, arXiv:quant-ph/0512247; generalized by Abeyesinghe, Devetak, Hayden & Winter, arXiv:quant-ph/0606225
> **Tags:** #trick #decoupling #quantum-shannon-theory #random-measurement #state-merging

## What it does

Destroys all correlations between a subsystem and a reference by applying a random unitary followed by a partial trace (or measurement), leaving the remaining system in a product state with the reference.

## The trick

Given a pure state $|\psi\rangle_{ABR}$, decompose $A$ into subsystems $A_1$ (to be discarded) and $A_2$ (to be kept). Apply a Haar-random unitary $U$ to $A$, then trace out $A_1$. The resulting state on $A_2 R$ satisfies:

$$\int_{U(A)} \left\| \sigma_{A_2 R}(U) - \sigma_{A_2}(U) \otimes \sigma_R(U) \right\|_1^2 \, dU \leq \frac{d_A d_R}{d_{A_1}^2} \left[ \operatorname{Tr}(\psi_{AR}^2) + \operatorname{Tr}(\psi_A^2)\operatorname{Tr}(\psi_R^2) \right]$$

where $\sigma_{A_2 R}(U) = \operatorname{Tr}_{A_1}[(U \otimes I_R)\psi_{AR}(U^\dagger \otimes I_R)]$.

In the i.i.d. setting ($n$ copies), this decouples whenever $\log d_{A_1} > \frac{1}{2}[H(A) + H(R) - H(AR)] = \frac{1}{2}I(A:R)$. Each qubit discarded reduces Alice's mutual information with the reference by at most 2 bits, so $\frac{1}{2}I(A:R)$ qubits is both necessary and sufficient.

The key insight: *destroying* correlation is easier than *creating* it. Since destruction is indiscriminate, random unitaries work.

## When to reach for it

- Proving quantum Shannon theorems (state merging, channel capacity, distributed compression). The approach: show a random encoding decouples the sender from the reference, then invoke Uhlmann's theorem for the receiver's reconstruction.
- Any setting where you need to argue that a subsystem becomes independent of an environment after a random operation.
- Constructing one-shot protocols for quantum state transfer or entanglement distillation.
- Showing that random circuits approximate unitary designs for information-theoretic tasks.

## Complexity

The random unitary is over $U(d_A)$, which requires $\Theta(d_A^2)$ parameters. For $n$ copies, this is exponential in $n$. However, later work (Abeyesinghe et al.) showed that random Clifford circuits suffice, reducing the implementation cost to $O(n^2)$ gates.

## Caveat

The result is existential — a random unitary works with high probability, but you may not know *which* unitary to use without exponential overhead. For practical protocols, you need efficient constructions (Clifford circuits, approximate $2$-designs). The one-shot bounds depend on purities rather than entropies, so the conversion to entropic quantities requires an i.i.d. structure or smooth entropy framework.

## Related notes
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes]] — original application to state merging
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]] — generalized decoupling theorem that unifies quantum Shannon theory
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]] — the approximate randomization result that prefigures decoupling
- [[Negative Conditional Entropy as Entanglement Gain]]
- [[Approximate Randomization with Few Unitaries]]
