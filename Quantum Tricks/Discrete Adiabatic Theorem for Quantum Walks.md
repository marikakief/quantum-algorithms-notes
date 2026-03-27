
> **Source:** Costa, An, Sanders, Su, Babbush, Berry, arXiv:2111.08152
> **Tags:** #trick #adiabatic #quantum-walk #qubitization #spectral-gap #error-bound

## What it does

Bounds the error of a discrete-time adiabatic evolution (a sequence of slowly varying [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk operators) in terms of the spectral gap, without requiring any continuous [[Hamiltonian simulation]].

## The trick

Given a sequence of unitary walk operators $\{W_T(n/T)\}_{n=0}^{T-1}$ whose eigenspaces evolve slowly, define the discrete evolution $U_T(s) = \prod_{n=0}^{sT-1} W_T(n/T)$ and let $U_T^A(s)$ be the ideal adiabatic evolution that perfectly tracks the eigenspace.

Let $c_k(s)$ bound the $k$th finite difference: $\|D^{(k)} W_T(s)\| \leq c_k(s)/T^k$.

Let $\check{\Delta}(s)$ be the spectral gap (minimum over neighboring steps).

Then for $T \geq \max_s(4\hat{c}_1(s)/\check{\Delta}(s))$:

$$\|U_T(s) - U_T^A(s)\| \leq \frac{12\hat{c}_1(0)}{T\check{\Delta}(0)^2} + \frac{12\hat{c}_1(s)}{T\check{\Delta}(s)^2} + \frac{6\hat{c}_1(s)}{T\check{\Delta}(s)} + O\left(\frac{1}{T^2}\right) \text{ sums}$$

The three sums (with $1/T^2$ per term, $T$ terms total) involve $\hat{c}_1^2/\check{\Delta}^3$, $\hat{c}_1^2/\check{\Delta}^2$, and $\hat{c}_2/\check{\Delta}^2$ — the first-order analogue of the continuous adiabatic theorem's dependence on $\|\dot{H}\|/\Delta^2$ and $\|\ddot{H}\|/\Delta^2$.

The key insight over the 1998 DKS result: the error scales as $O(\kappa/T)$ for a gap $\Delta \sim 1/\kappa$, not just $O(1/T)$ with unknown constant. This explicit gap tracking is what makes the theorem algorithmically useful.

The proof uses the wave operator $\Omega_T = (U_T^A)^\dagger U_T$, a Volterra-type recursion, and a discrete summation-by-parts formula to separate diagonal (easily bounded) and off-diagonal (harder) contributions.

## When to reach for it

- Any time you want to adiabatically prepare a ground state using a [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized walk]] instead of continuous [[Hamiltonian simulation]].
- When the Dyson series overhead ($\log$ factors from truncation) is the bottleneck.
- Quantum optimization via adiabatic paths where the cost function has a known gap.
- Replacing the [[Stroboscopic Adiabatic Walk]] with a rigorously bounded version.

## Complexity

$T = O(c_1 / (\varepsilon \cdot \Delta_{\min}))$ walk steps for $\varepsilon$-accurate eigenstate tracking, where $c_1$ is the maximum rate of change of the walk operator and $\Delta_{\min}$ is the minimum gap. For the QLSP with gap $\sim 1/\kappa$, this gives $T = O(\kappa/\varepsilon)$.

## Caveat

The constant factors in the analytical bound are large ($\sim 5632$ for the Hermitian case, vs. $\sim 638$ numerically). The first form of the theorem (Theorem 15 in the paper) is tighter but extremely complicated — the simplified form (Theorem 3) sacrifices about a factor of 2 in the constants for readability.

The theorem requires the walk operators to have bounded first *and* second finite differences. If $W_T(s)$ changes sharply (e.g., near a phase transition), $c_2(s)$ blows up and the bound becomes vacuous.

## Related notes
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — Numerically validates that the constant factor is ~1,500× tighter than the analytical bound
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — Uses the discrete adiabatic theorem to show [[Product Formulas]] time steps can be $\varepsilon$-independent; extends to boundary cancellation regime
- [[Discrete Adiabatic View of Numerical Integrators]] — The conceptual framework that makes this theorem the right tool for analysing digitised AQC
- [[Stroboscopic Adiabatic Walk]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
