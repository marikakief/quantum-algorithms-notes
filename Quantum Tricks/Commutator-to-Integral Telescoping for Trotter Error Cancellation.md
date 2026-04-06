# Commutator-to-Integral Telescoping for Trotter Error Cancellation

> **Source:** Tran, Chu, Su, Childs, Gorshkov, arXiv:1912.11047
> **Tags:** #trick #trotter #product-formulas #error-cancellation #lattice

## What it does

Converts a sum of conjugated commutator errors across Trotter steps into a telescoped integral that evaluates to bounded norm, reducing the total error by a factor of $t$ compared to the triangle inequality.

## The trick

When the first-order [[Product Formulas|product formula]] is applied over $r$ steps, the total error is (to leading order):

$$
\Delta \approx \sum_{j=0}^{r-1} U_{t/r}^j \, \delta \, U_{t/r}^{-j}
$$

where $\delta \approx \frac{t^2}{2r^2} [H, H_2]$ is the per-step error. The triangle inequality gives $\|\Delta\| \le r\|\delta\| = O(nt^2/r)$.

Instead, use the identity:

$$
U_t A U_{-t} - A = -i \int_0^t U_x [H, A] U_{-x} \, dx.
$$

Replace the discrete sum with the integral approximation:

$$
\sum_{j=0}^{r-1} U_{t/r}^j [H, A] U_{t/r}^{-j} \approx \frac{r}{t} \int_0^t U_x [H, A] U_{-x} \, dx = \frac{r}{t} (U_{-t} A U_t - A).
$$

Since $\|U_{-t} A U_t - A\| \le 2\|A\|$ and $A = \frac{t^2}{2r^2} H_2$ with $\|H_2\| = O(n)$:

$$
\|\Delta\| \le \frac{r}{t} \cdot 2 \cdot \frac{t^2}{2r^2} \|H_2\| = O\!\left(\frac{nt}{r}\right).
$$

The key: the sum of evenly-spaced rotations of an operator telescopes via the integral identity. The rotated terms interfere destructively, just as $\sum_{j=0}^{r-1} e^{ij\theta}$ is $O(1)$ rather than $O(r)$.

## When to reach for it

- Bounding total error of PF1 on lattice systems where $H = H_1 + H_2$ with each part internally commuting
- Any setting where you sum a quantity conjugated by evenly-spaced time evolutions and expect cancellation
- When the per-step error has the form $[H, S]$ (a commutator with the full Hamiltonian)

## Complexity

No additional computational cost — this is a tighter *analysis* of the same algorithm. The PF1 gate count improves from $O(n^2 t^2/\varepsilon)$ to $O(n^2 t/\varepsilon)$ for lattice systems.

## Caveat

- Requires that the per-step error decompose as $[H, S_k] + V_k$ where $V_k$ is suitably small. For PF1 with a two-part commuting decomposition, $V_2 = 0$ exactly, making this clean. For higher-order formulas (PF2, PF4), the error structure is more complex and the telescoping doesn't apply as directly.
- The integral approximation to the discrete sum introduces its own error, bounded in [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes|Tran et al. (2020)]] via a convergent series expansion.
- Only proven under the condition $nt^2/r < \text{const}$, though numerics suggest broader validity.

## Related notes

- [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes]]
- [[Inductive Commutator-Remainder Decomposition]]
- [[Trotter Commutator-Scaling Bound]]
- [[Product-Formula Error Representation Switching]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]]
