# Adaptive Kernel Parameter for LCHS

> **Source:** An, Childs, Lin, arXiv:2312.03916 (Corollary 19)
> **Tags:** #trick #LCHS #kernel #optimisation

## What it does

Optimises the kernel parameter $\beta$ in the [[Near-Optimal Hardy-Space Kernel for LCHS|LCHS kernel family]] as a function of the target precision $\varepsilon$, trading a small overhead in state-preparation cost for a cleaner matrix-query scaling.

## The trick

The LCHS kernel $f_\beta(k) = (1/C_\beta) e^{(1+ik)^\beta}$ has two competing effects as $\beta$ increases toward 1:
- **Better truncation:** The $k$-integral truncation point scales as $K = O((\log 1/\varepsilon)^{1/\beta})$, so larger $\beta$ means smaller $K$.
- **Worse constants:** The $L^1$ norm $\|f_\beta/(1-i\cdot)\|_{L^1}$ grows as $\beta \to 1$, increasing the [[Linear Combination of Unitaries (LCU)|LCU]] normalisation and hence the state-preparation cost.

Choose $\beta = 1 - O(1/\log\log(1/\varepsilon))$. Then:

$$(\log 1/\varepsilon)^{1/\beta} = (\log 1/\varepsilon)^{1 + O(1/\log\log(1/\varepsilon))} = O((\log 1/\varepsilon)^{1+o(1)}),$$

so the matrix-query cost becomes $\tilde{O}(\alpha_A T \cdot (\log 1/\varepsilon)^2)$ for the time-dependent case and $\tilde{O}(\alpha_A T \cdot (\log 1/\varepsilon)^{1+o(1)})$ for the time-independent case. The state-preparation cost picks up only a $\log\log(1/\varepsilon)$ multiplicative factor.

## When to reach for it

When using [[LCHS Kernel for Non-Unitary Dynamics|LCHS]] or [[Laplace-Transform LCU Lifting for Eigenvalue Transforms|Lap-LCHS]] and you want to report the cleanest asymptotic complexity without leaving a free parameter.

## Complexity

Matrix queries: $\tilde{O}(\alpha_A T \cdot (\log 1/\varepsilon)^2)$ (time-dependent).
State-prep: $O((\|u_0\| + \|b\|_{L^1})/\|u(T)\| \cdot \log\log(1/\varepsilon))$.

## Caveat

The choice $\beta = 1 - O(1/\log\log(1/\varepsilon))$ is asymptotically motivated. For concrete parameter regimes with moderate $\varepsilon$, a fixed $\beta$ (say $\beta = 0.9$) might give better constants.

## Related notes

- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[Near-Optimal Hardy-Space Kernel for LCHS]]
- [[Phragmén–Lindelöf Decay Barrier for Analytic Kernels]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
