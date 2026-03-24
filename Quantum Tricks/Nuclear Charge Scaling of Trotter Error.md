# Nuclear Charge Scaling of Trotter Error

> **Source:** Babbush, McClean, Wecker, Aspuru-Guzik, Wiebe, arXiv:1410.8159 (PRA 2015)
> **Tags:** #trick #trotter-error #quantum-chemistry #scaling #nuclear-charge

## What it does

Identifies the maximum nuclear charge $Z_{\max}$ — not the number of spin orbitals $N$ — as the primary driver of Trotter-Suzuki error in quantum chemistry simulation.

## The trick

In an atomic orbital basis, the one-electron integrals scale as $|h_{pq}| = \Theta(Z^2)$ and the two-electron integrals as $|h_{pqrs}| = \Theta(Z)$. This follows from rescaling $r \to \rho = Zr$ in the hydrogenic wavefunctions — factors of $Z$ from the volume elements cancel with normalisation, leaving the nuclear Coulomb potential $\propto Z^2$ and electron-electron repulsion $\propto Z$.

The leading-order second-order Trotter error operator involves double commutators of Hamiltonian terms:

$$V^{(1)} = -\frac{\Delta t^2}{12}\sum_{\alpha \leq \beta}\sum_{\gamma < \beta}\left[H_\alpha\left(1 - \frac{\delta_{\alpha,\beta}}{2}\right),\, [H_\beta, H_\gamma]\right]$$

Since the one-body terms dominate ($\Theta(Z^2)$ vs $\Theta(Z)$), the double commutator gives $\|V^{(1)}\| \in O(N^4 Z_{\max}^6 + N^{10} Z_{\max}^3)$. Numerically, this fits as $\sim Z_{\max}^6$ with $r^2 = 0.994$ in the local basis.

## When to reach for it

- Estimating Trotter step counts for quantum chemistry simulations — use $Z_{\max}$ rather than $N$ as the scaling parameter.
- Choosing which molecules benefit most from higher-order Trotter formulas or non-Trotter methods ([[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]], [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]): heavy atoms with large $Z$ pay the biggest Trotter penalty.
- Understanding why light-element chemistry (H, He, Li) is relatively cheap to simulate with product formulas.

## Complexity

Trotter error norm: $O(Z_{\max}^6)$ in the local basis, $\sim Z_{\max}^5$ in canonical (HF) basis. This determines the number of Trotter steps $\mu$ needed for a given accuracy target $\varepsilon$.

## Caveat

The $Z^6$ scaling is for the *norm* of the error operator — the actual ground-state error can be dramatically smaller (by up to 16 orders of magnitude for closed-shell atoms). Also only validated numerically for second-order Trotter in minimal basis sets ($\leq 20$ spin orbitals). The scaling in the canonical basis is empirically closer to $Z^5$ and may have different structure at larger $N$.

## Related notes
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Finite Nested-Commutator Expansion]]
- [[Classical Ansatz Error Subtraction for Quantum Simulation]]
