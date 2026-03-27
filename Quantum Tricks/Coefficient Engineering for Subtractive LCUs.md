
> **Tags:** #trick #LCU #coefficient-design #multi-product-formula
> **Source:** Childs–Wiebe arXiv:1202.5822

## What it does
Improves success probability of LCUs with negative coefficients by tuning decomposition parameters so one positive branch dominates subtraction.

## The trick
For target $\sum_q C_q U_q$ (an [[Linear Combination of Unitaries (LCU)|LCU]]), split positive/negative parts and implement $\kappa A - B$ with
$$\kappa = \frac{\sum_{C_q>0} C_q}{\sum_{C_q<0}|C_q|}$$

Subtractive failure scales like $\sim 4\kappa/(\kappa+1)^2$. To suppress it, engineer coefficients so effective subtraction is favorable (dominant branch + small correction terms).

In multi-[[Product Formulas]]s, choose one step parameter very large so its coefficient dominates, improving subtraction success while keeping approximation order constraints. Related: [[Failure Branch Inversion via E(-λ)E(λ)]] for recovering from failed subtraction steps.

## When to reach for it
- Any [[Linear Combination of Unitaries (LCU)|LCU]] with mixed signs (common in high-order formulas)
- Multi-product integrators and extrapolation-style constructions
- Designing practical postselected implementations from formal series with cancellations

## Complexity impact
Can turn impractical exponentially small success into polynomially large success by reshaping coefficient spectrum.

## Caveat
Trade-off: coefficient engineering may increase circuit depth/cost in dominant branches. Must balance success probability vs gate count.

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
