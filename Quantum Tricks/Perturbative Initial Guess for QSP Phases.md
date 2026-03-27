
> **Source:** Dong, Meng, Whaley & Lin, arXiv:2002.11649, Phys. Rev. A **103**, 042419 (2021)
> **Tags:** #trick #QSP #phase-factors #optimisation #perturbation #scaling-and-squaring

## What it does

Provides an explicit, closed-form initial guess for [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] phase factors that's close enough to a global optimum for gradient methods to converge reliably. Combined with scaling-and-squaring, it handles target polynomials of any norm.

## The trick

**Small-norm regime** ($\|f\|_\infty \ll 1$): The [[Symmetry Reduction for Phase Factor Uniqueness|symmetric]] QSP phases $\Phi$ are related to the Chebyshev coefficients of the target polynomial $f(x) = \sum_j \hat{f}_j T_j(x)$ by a perturbative expansion. At leading order:

$$\tilde{\phi}_k \approx \frac{1}{2}\hat{f}_k, \quad k = 0, \ldots, \lceil d/2 \rceil - 1,$$

where $\tilde{\phi}_k = \phi_k - \frac{\pi}{4}(\delta_{k,0} + \delta_{k,d})$ is the shifted phase (relative to the trivial QSP sequence $\Phi_0 = (\pi/4, 0, \ldots, 0, \pi/4)$).

**Large-norm regime** ($\|f\|_\infty \approx 1$): Use **scaling-and-squaring**:
1. Scale down: solve for $f_\delta(x) = \delta \cdot f(x)$ with $\delta$ small enough that the perturbative guess works
2. Double: use the Chebyshev identity $2y^2 - 1 = T_2(y)$ to compose the QSP circuit with itself, effectively squaring the polynomial while tracking the complementary polynomial
3. Repeat until the target norm is reached

Each doubling step requires a fresh optimisation solve, but each converges fast because the initial guess (from the previous step's output) is already close.

## When to reach for it

- Initialising any optimisation-based QSP phase-factor solver (QSPPACK, pyqsp, etc.)
- When the [[Recursive Phase Extraction from Polynomial Coefficients|recursive extraction]] method fails due to numerical instability at high degree
- As a diagnostic: if the perturbative phases give a poor approximation even at small $\|f\|$, something is wrong with the target polynomial

## Complexity

Computing the initial guess is $O(d \log d)$ (FFT for Chebyshev coefficients). The scaling-and-squaring adds $O(\log(1/\delta))$ optimisation runs, each on a degree-$d$ problem.

## Caveat

The perturbative expansion is only accurate when $\|f\|_\infty = O(d^{-1})$ — this is proved rigorously in [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang-Dong-Lin (2022)]]. In practice, the initial guess works well for $\|f\|_\infty$ up to about 0.9, well beyond the provable regime. For $\|f\|_\infty$ very close to 1, you need the scaling-and-squaring route.

## Related notes
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]]
- [[Chebyshev-Node Loss for QSP Phase Optimisation]]
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]
- [[Symmetry Reduction for Phase Factor Uniqueness]]
- [[Recursive Phase Extraction from Polynomial Coefficients]]
- [[Maximal Solution Selection via Mahler Measure]]
