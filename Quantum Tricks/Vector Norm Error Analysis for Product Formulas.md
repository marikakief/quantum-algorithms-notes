
> **Source:** Dong An, Di Fang, and Jin-Peng Liu, arXiv:2012.13105 (Quantum 2021); extends Jahnke-Lubich (BIT 2000)
> **Tags:** #trick #trotter #product-formulas #vector-norm #unbounded-operators #PDE

## What it does

Replaces the operator-norm Trotter error bound $\|S(h) - U(h)\| \leq C h^{p+1} \|H\|^{p+1}$ with a vector-norm bound $\|S(h)v - U(h)v\| \leq \tilde{C} h^{p+1} (\|Hv\| + \|v\|)$ where $\tilde{C}$ is independent of $\|H\|$. For states that are smooth relative to the Hamiltonian ($\|Hv\| \ll \|H\|\|v\|$), this can be a dramatic improvement.

## The trick

Write the one-step Trotter error using the **variation-of-constants formula**:

$$S(h)v - U(h)v = \int_0^h U(h,s) \cdot [\text{local error generator}] \cdot S(s)v\, ds.$$

The local error generator involves commutators $[H_1, H_2]$ (for operator-norm bounds, you'd bound these in operator norm). Instead, evaluate the integrand *applied to the vector* $S(s)v$:

$$\|[H_1, H_2] S(s)v\| = \|H_1 H_2 S(s)v - H_2 H_1 S(s)v\|.$$

For $H_1 = -\Delta$ and smooth $v$, $\|H_1 S(s)v\|$ stays bounded (by stability of the evolution), so $\|H_1 H_2 S(s)v\| \leq \|H_1\| \cdot \|H_2 S(s)v\|$ can be replaced by $\|H_2\| \cdot \|H_1 S(s)v\| \leq \|H_2\| \cdot (\tilde{C}_1 \|H_1 v\| + \tilde{C}_2 \|v\|)$.

The critical ingredient is **Assumption 1**: for self-adjoint $A$,

$$\|A e^{-isA} v\| \leq C_1 \|Av\| + C_2 \|v\|$$

holds trivially by spectral theory (since $A$ commutes with $e^{-isA}$, giving $\|Ae^{-isA}v\| = \|Av\|$ exactly). For the cross-term $\|Ae^{-isB}v\|$ with $B \neq A$, verification is case-specific but holds for differential + multiplication operator pairs under smoothness.

## When to reach for it

- Simulating PDEs on fine grids where the Hamiltonian norm grows with grid size but the solution stays smooth.
- Any [[Hamiltonian simulation]] where the initial state sits in a low-energy subspace relative to the dominant term.
- Comparing Trotter methods fairly against post-Trotter methods: the operator-norm comparison may overstate Trotter's cost for physical initial states.

## Complexity

For the Schrödinger equation $H = -\Delta + V(x)$ on $n$ grid points:
- Operator-norm bound: $L = O(n^2 T^2/\varepsilon)$ steps for first-order Trotter
- **Vector-norm bound:** $L = O(T^2/\varepsilon)$ — independent of $n$

The cost per step still involves $e^{-i\Delta h}$ and $e^{-iVh}$, which may have their own $n$-dependence, but the *number of steps* doesn't grow.

## Caveat

- Only proved for $p=1,2$ (first and second-order Trotter). Higher-order Suzuki should work but isn't shown.
- The bound involves $\sup_t \|H_1|\psi(t)\rangle\|$ — a property of the exact solution you're trying to compute. In practice, you need an a priori estimate or physical argument that the solution stays regular.
- If the solution develops fine-scale features (e.g., shock-like behavior), the vector norm grows and the bound degrades.
- Only two-term splittings $H = f_1(t)H_1 + f_2(t)H_2$ are treated.

## Related notes
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes]]
- [[Trotter Commutator-Scaling Bound]]
- [[Generalised Trotter for Time-Dependent Hamiltonians]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Superconvergence via Pseudo-Differential Commutator Cancellation]]
