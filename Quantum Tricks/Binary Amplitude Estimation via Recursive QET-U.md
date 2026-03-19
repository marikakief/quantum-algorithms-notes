# Binary Amplitude Estimation via Recursive QET-U

> **Source:** Yulong Dong, Lin Lin, and Yu Tong, arXiv:2204.05955 (PRX Quantum 2022), Lemma 12 & Appendix D
> **Tags:** #trick #amplitude-estimation #QET-U #ancilla-efficient

## What it does

Solves the **binary amplitude estimation** problem — distinguishing whether a success amplitude $A$ is above $\gamma_2$ or below $\gamma_1$ — using $O((\gamma_2 - \gamma_1)^{-1}\log(\vartheta^{-1}))$ queries and only **one additional ancilla qubit**. Standard amplitude estimation needs $O(\log \gamma^{-1})$ ancilla qubits.

## The trick

Given a unitary $W$ with success amplitude $A = \|(\langle 0| \otimes I)W(|0\rangle|0^n\rangle)\|$, construct the Grover iterate $R_0 R_1$ from the usual reflections. This has eigenvalues $e^{\pm 2i\arccos(A)}$.

The key move: treat $R_0 R_1 = e^{-iL}$ as a **Hamiltonian evolution**, where $L$ has eigenvalues $\pm 2\arccos(A)$. Now apply [[QET-U — Eigenvalue Transformation via Hamiltonian Evolution|QET-U]] to $R_0 R_1$ with a polynomial $P(x)$ that approximates the sign function:
- $P(x) \geq 1 - \delta$ for $x \in [\gamma_2, 1]$
- $|P(x)| \leq \delta$ for $x \in [0, \gamma_1]$

Then measure $|P(A)|^2$ via Monte Carlo sampling on the ancilla. If $A \geq \gamma_2$, the measurement probability is $\geq (1-\delta)^2$; if $A \leq \gamma_1$, it's $\leq \delta^2$. With $\delta = 1/4$, majority voting over $O(\log \vartheta^{-1})$ rounds gives success probability $\geq 1 - \vartheta$.

The polynomial degree is $O((\gamma_2 - \gamma_1)^{-1}\log(\delta^{-1}))$, giving the query complexity.

## When to reach for it

- Anywhere standard [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]] is used but ancilla qubits are scarce.
- Inside early fault-tolerant algorithms where you need to distinguish "large overlap" from "small overlap" without QPE.
- As a subroutine for binary search over eigenvalues (the fuzzy bisection pattern in ground-state energy estimation).

## Complexity

- **Queries to $W$:** $O((\gamma_2 - \gamma_1)^{-1}\log(\vartheta^{-1}))$
- **Additional ancilla qubits:** 1 (for QET-U on the walk operator) + 1 (for the reflection), total 2 beyond what $W$ uses
- **Multi-qubit control:** Need $(n+2)$-qubit Toffoli for the reflection operator $R_1$ — this is "low-level" MQC

## Caveat

- Requires implementing the Grover reflection operator $2|0^{n+1}\rangle\langle 0^{n+1}| - I$, which needs controlled gates scaling linearly in $n$. Not truly control-free.
- Only decides a binary question (above/below threshold). For full amplitude estimation with precision $\varepsilon$, combine with binary search.
- The reflection implementation adds $O(n)$ two-qubit gates per query, which can dominate for large system sizes.

## Related notes
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]
- [[QET-U — Eigenvalue Transformation via Hamiltonian Evolution]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Approximate CDF from Hadamard Tests]]
