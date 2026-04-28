# k-Uniformity Convergence to Average-Case Error

> **Source:** Zhao, Zhou, Childs, arXiv:2406.02379
> **Tags:** #trick #hamiltonian-simulation #trotter #product-formulas #average-case #k-uniform #entanglement

## What it does

Gives a clean sufficient condition under which a fixed input state has the same Trotter-error scaling as the [[Frobenius-Norm Average-Case Error Bound|average case]]: enough local reduced density matrices must already be close to maximally mixed.

## The trick

A pure $N$-qubit state is $\Delta$-approximately $k$-uniform if every $k$-qubit reduced density matrix is within trace distance $\Delta$ of maximally mixed.

In this paper the leading Trotter error is decomposed as $E = \sum_j E_j$. If

$$
k \ge 2\max_j w(E_j)
\qquad\text{and}\qquad
\sqrt{\Delta} \le \frac{\|E\|_F}{\sum_j \|E_j\|},
$$

then all reduced states on supports of $E_j^\dagger E_{j'}$ are close enough to maximally mixed that the correction term in the entanglement-based bound is absorbed into the Frobenius piece. Therefore

$$
\|(U_0-\mathcal U_p)|\psi\rangle\| = O(\|E\|_F\,\delta t^{p+1}).
$$

So approximate local pseudorandomness on the support size of the leading error terms is enough to force average-case scaling.

For geometrically local shallow-circuit states, the light-cone refinement weakens this to roughly $k \ge \max_j w(E_j)$.

## When to reach for it

- When you can certify that evolved states are locally thermalized or look random on constant-size subsystems
- When you want a simple sufficient condition for average-case Trotter scaling without averaging over an ensemble
- When comparing thermalizing and non-thermalizing dynamics in [[Hamiltonian simulation]]

## Complexity

No algorithmic overhead from the theorem itself. It is a structural certificate.

## Caveat

- The threshold is tied to the support size of the leading error operators, not just to formula order.
- Certifying approximate $k$-uniformity can be expensive in practice.
- This is sufficient, not necessary. Many states can perform near the average case without satisfying such a strong uniformity condition.

## Related notes

- [[Entanglement Accelerates Quantum Simulation (Zhao-Zhou-Childs 2025) — Paper Notes]]
- [[Entanglement-Entropy-Dependent Trotter Error Bound]]
- [[Frobenius-Norm Average-Case Error Bound]]
- [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]]
