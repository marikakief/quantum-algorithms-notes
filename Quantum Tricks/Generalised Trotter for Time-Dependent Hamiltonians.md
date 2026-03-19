# Generalised Trotter for Time-Dependent Hamiltonians

> **Source:** Huyghebaert and De Raedt, J. Phys. A 23 (1990); rigorously analysed by Dong An, Di Fang, and Jin-Peng Liu, arXiv:2012.13105 (Quantum 2021)
> **Tags:** #trick #trotter #time-dependent #commutator-scaling #hamiltonian-simulation

## What it does

Replaces the "freeze and multiply" standard Trotter approximation for time-dependent Hamiltonians with an "integrate and exponentiate" version that eliminates the leading-order error from time-variation of the coefficients.

## The trick

For $H(t) = f_1(t)H_1 + f_2(t)H_2$, the standard Trotter on $[t_l, t_{l+1}]$ freezes $f_j$ at $t_l$:

$$U_{\text{s},1}(t_{l+1}, t_l) = e^{-if_2(t_l)H_2 h} \cdot e^{-if_1(t_l)H_1 h}.$$

The generalised Trotter integrates instead:

$$U_{\text{g},1}(t_{l+1}, t_l) = \exp\!\left(-iH_2\int_{t_l}^{t_{l+1}} f_2(\tau)\,d\tau\right) \cdot \exp\!\left(-iH_1\int_{t_l}^{t_{l+1}} f_1(\tau)\,d\tau\right).$$

This absorbs the time-variation of $f_j$ into the integration, so the leading-order error comes *only from non-commutativity of $H_1$ and $H_2$*, not from derivatives $f_j'$.

**Concrete comparison of leading local errors (first order):**

| Method | Leading error | Depends on $f'$? |
|---|---|---|
| Standard | $\frac{1}{2}\|f_1'\|_\infty\|H_1\|h^2 + \frac{1}{2}\|f_2'\|_\infty\|H_2\|h^2 + \frac{1}{2}\|f_1\|_\infty\|f_2\|_\infty\|[H_1,H_2]\|h^2$ | Yes |
| Generalised | $\frac{1}{2}\|f_1\|_\infty\|f_2\|_\infty\|[H_1,H_2]\|h^2$ | No |

The generalised version has pure [[Trotter Commutator-Scaling Bound|commutator scaling]] for time-dependent Hamiltonians, matching what you get for time-independent ones.

## When to reach for it

- Time-dependent Hamiltonian simulation with rapidly varying coefficients (large $f'$).
- Situations where $[H_1, H_2]$ is small but $\|H_1\|$ and $f_1'$ are large — the standard Trotter overpays while the generalised version doesn't see the derivatives.
- Quantum control problems where $f_j(t)$ are control pulses with fast modulation.
- As a drop-in replacement for standard Trotter in PDE simulations.

## Complexity

Same gate structure as standard Trotter — each step still applies $e^{-iH_j \theta_j}$ — but now $\theta_j = \int f_j(\tau)\,d\tau$ instead of $f_j(t_l) \cdot h$. The integration can be precomputed classically, so the quantum circuit is identical.

Combined with [[Vector Norm Error Analysis for Product Formulas|vector-norm scaling]], first-order generalised Trotter achieves:

$$L = O\!\left(\frac{T^2}{\varepsilon}\sup_t \sqrt{\|\psi_0\|\|H_1|\psi(t)\rangle\|}\right)$$

— the geometric mean instead of $\|H_1|\psi(t)\rangle\|$, which is even better.

## Caveat

- Requires computing $\int_{t_l}^{t_{l+1}} f_j(\tau)\,d\tau$ classically. For smooth $f_j$ this is standard; for complicated time-dependence it adds classical preprocessing.
- The advantage over standard Trotter vanishes when $f_j$ are constant (time-independent case).
- Only analysed for two-term splittings.

## Related notes
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — first major application of this trick to prove BQP equivalence of time-dependent Hamiltonian models
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes]]
- [[Trotter Commutator-Scaling Bound]]
- [[Vector Norm Error Analysis for Product Formulas]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Magnus-LCU for Time-Averaged Hamiltonian Block-Encoding]]
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]]
