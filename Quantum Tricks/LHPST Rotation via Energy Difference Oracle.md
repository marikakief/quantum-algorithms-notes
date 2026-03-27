
> **Source:** Sanders, Berry, Costa, Tessler, Wiebe, Gidney, Neven, Babbush, arXiv:2007.07391 (improving on Lemieux, Heim, Poulin, Svore, Troyer 2020)
> **Tags:** #trick #quantum-simulated-annealing #quantum-walk #QROM #dense-hamiltonian

## What it does
Implements the controlled rotation step in qubitized quantum simulated annealing (the $B$ operation in the LHPST walk) for arbitrary-connectivity Hamiltonians, replacing a procedure whose cost is exponential in connectivity with one that scales polynomially.

## The trick
In the LHPST quantum walk for simulated annealing, the $B$ operation rotates an ancilla qubit by $\sqrt{p_{x,x_j}}$ (the transition probability for flipping bit $j$ of state $x$). LHPST's original method: classically precompute the rotation angle for every basis state of the qubits that influence the transition probability, and access via multiply-controlled Toffolis. Cost: $O(N \cdot 2^{|N_j|})$, where $|N_j|$ is the number of qubits the energy difference depends on. For dense Hamiltonians (SK, LABS, general QUBO), $|N_j| = \Theta(N)$ — exponential blowup.

The replacement:

1. Compute the energy difference $E_x - E_{x_j}$ using the energy difference oracle $O_{\text{diff}}$. Cost: $C_{\text{diff}}$.
2. Evaluate $\arcsin(\sqrt{p_{x,x_j}})$ using [[Adaptive QROM for Function Evaluation|QROM-based piecewise linear interpolation]]. Cost: $C_{\text{fun}}$.
3. Perform the rotation using the [[Phase Gradient State for Controlled Rotations|phase gradient state]]. Cost: $b_{\text{sm}}$ Toffolis.
4. Uncompute steps 2 and 1.

Further optimization: retain the energy difference register during the $F$ operation (controlled bit flip). Since $F$ just flips bit $j$, it reverses the sign of $E_x - E_{x_j}$. By tracking the sign bit explicitly:
- If $E_{x_j} < E_x$: apply X to ancilla C (transition is always accepted).
- Otherwise: rotate by the computed angle.

This halves the cost from $4(C_{\text{diff}} + C_{\text{fun}})$ to $2(C_{\text{diff}} + C_{\text{fun}}) + 2b_{\text{dif}} + O(1)$ per walk step.

## When to reach for it
- Quantum simulated annealing on any optimization problem with dense (high-connectivity) Hamiltonians.
- Any quantum walk algorithm requiring transition probability computations where the probability depends on many qubits.
- Specifically: SK model, LABS, general QUBO, and any Ising model that isn't sparse.

## Complexity
Per walk step (LHPST with this improvement):
$$2C_{\text{diff}} + 2C_{\text{fun}} + N + 2b_{\text{dif}} + 9\log N + O(1) \text{ Toffolis}$$

For SK: $5N + 2(b_{\text{sm}} + b_{\text{fun}})^2 + 11\log N + O(b_{\text{sm}} \log b_{\text{sm}})$.

Compare to LHPST original for SK ($|N_j| = N-1$): $O(N \cdot 2^N)$ — completely impractical.

## Caveat
Still requires computing the energy difference, which costs $O(N)$ for 2-local Hamiltonians and $O(N^2)$ for LABS. The QROM interpolation introduces approximation error in the rotation angle, but numerical tests show negligible impact on optimization quality. The sign-tracking optimization requires the energy difference to be represented as a signed integer, adding a $b_{\text{dif}}$-cost conversion from two's complement.

## Related notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[Adaptive QROM for Function Evaluation]]
- [[Phase Gradient State for Controlled Rotations]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
