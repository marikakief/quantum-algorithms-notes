# Interaction-Picture Split for Product Formulas

> **Tags:** #trick #interaction-picture #trotter
> **Source:** arXiv:1912.08854; related to Low–Wiebe (arXiv:1805.00675) and prior interaction-picture simulation work

## What it does

Writes $H = A + B$ where $A$ is large but cheap to simulate exactly, and $B$ is harder but smaller. Move to the [[Interaction-Picture Norm Reduction|interaction picture]] of $A$: effective cost then tracks $B$ rather than $A + B$.

Formally: instead of simulating $e^{-i(A+B)t}$ directly, use
$$
e^{-i(A+B)t} = e^{-iAt} \cdot \mathcal{T}\exp\!\left(-i\int_0^t e^{iAs}B\,e^{-iAs}\,ds\right),
$$
and simulate the second factor. For product formulas, this means applying frame rotations $e^{-iA\cdot}$ between simulation steps of $B$ in the rotating frame.

## When to reach for it

Most useful when:
- $A$ is a dominant term (kinetic energy, mean-field part, free-theory piece) with a known cheap simulation,
- $B$ is the interaction term — smaller norm but harder to integrate directly,
- error/cost should scale with $\|B\|$ not $\|A + B\|$.

This can make the effective $\tilde{\alpha}_{\mathrm{comm}}$ much smaller, since you're Trotterizing only the interaction part while the free part is handled exactly.

## Complexity

Can reduce prefactors substantially when $\|B\| \ll \|A\|$. In chemistry contexts, moving the kinetic term into the frame and Trotterizing only the Coulomb terms is a key practical optimization.

## Caveat

Requires efficient controlled frame evolutions (i.e., $e^{-iAt}$ applied coherently with time parameter controlled). For product formulas specifically, this means the frame rotation overhead must be cheap.

## Related Paper Notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
