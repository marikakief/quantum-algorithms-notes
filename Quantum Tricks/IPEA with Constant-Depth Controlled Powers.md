# IPEA with Constant-Depth Controlled Powers

> **Source:** Lanyon, Whitfield, Aspuru-Guzik, White et al., Nature Chemistry 2, 106 (2010); arXiv:0905.0887
> **Tags:** #trick #phase-estimation #quantum-chemistry #circuit-depth

## What it does
Achieves arbitrary-precision phase estimation without increasing circuit depth per iteration, by exploiting a parametric decomposition of the time-evolution operator.

## The trick

When the Hamiltonian block is small enough to admit a closed-form unitary decomposition:

$$\hat{U} = e^{i\alpha} \hat{R}_y(\beta) \hat{R}_z(\gamma) \hat{R}_y(-\beta)$$

the $j$-th power is simply:

$$\hat{U}^j = e^{ij\alpha} \hat{R}_y(\beta) \hat{R}_z(j\gamma) \hat{R}_y(-\beta)$$

The power $j = 2^{k-1}$ enters only through the rotation angles $\alpha$ and $\gamma$, not through the circuit structure. So implementing controlled-$\hat{U}^{2^{k-1}}$ for the IPEA costs the same number of gates regardless of $k$.

This means the $k$-th iteration of the IPEA has the same gate complexity and the same error rate as the first iteration. Gate errors don't accumulate with increasing precision — each additional bit costs exactly one more round of the same circuit.

## When to reach for it

- Quantum chemistry on very small systems (2×2 Hamiltonian blocks that fit in a single qubit)
- Any eigenvalue problem where the evolution operator has an analytic parametric form as a function of time
- Teaching/demonstration contexts where you want to show high-precision IPEA without worrying about depth scaling

## Complexity

- **Gates per iteration:** constant (independent of $k$)
- **Total for $m$ bits:** $O(m)$ gates, each with the same error rate
- **Samples per bit:** $n$ repetitions for majority vote → $O(mn)$ total shots

## Caveat

Only works when the unitary has a parametric decomposition where the power enters through angles rather than circuit depth. For generic multi-qubit Hamiltonians, implementing $\hat{U}^{2^{k-1}}$ requires composing $2^{k-1}$ copies of the base circuit (or using Trotterization with correspondingly longer evolution time), so the gate count and error grow with $k$. This is the typical situation for any molecule larger than the single-qubit blocks in minimal-basis H$_2$.

## Related notes
- [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes]]
- [[Recursive Phase Estimation for Qubit-Efficient Readout]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
