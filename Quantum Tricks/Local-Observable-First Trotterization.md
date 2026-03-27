
> **Tags:** #trick #locality #trotter #observables
> **Source:** Childs, Su, Tran, Wiebe, Zhu, arXiv:1912.08854

## What it does

When only local observables are needed (e.g., magnetization on a small subsystem), the simulation cost can be tied to the effective light cone or relevant neighborhood rather than the full system size — sometimes system-size independent for long-range models.

## The trick

Use locality of the operator being measured and the Lieb–Robinson light cone (or the algebraic cancellation in the commutator bound) to truncate the [[Order-Condition Cancellation in Product Formulas|product formula]] to the relevant subsystem. Terms outside the light cone at the given simulation time contribute negligibly to the target expectation.

Concretely, if the observable $O$ is supported on a region $R$, then at time $t$ only terms within a ball of radius $\sim vt$ (Lieb–Robinson velocity $v$) around $R$ matter. For power-law interactions, the effective radius may grow slowly enough to keep costs manageable even as $n \to \infty$.

## When to reach for it

- Large lattice simulations where only local measurements matter.
- Power-law interaction models where the effective light cone is narrow.
- Comparing against black-box global simulation costs (the right comparison for local tasks).

## Complexity

Can make gate costs effectively system-size independent in favorable decay regimes. This is one of the places where the commutator bound from 1912.08854 directly implies new results: $\tilde{\alpha}_{\mathrm{comm}}$ can be evaluated on the truncated system.

## Caveat

Requires rigorous light-cone / truncation error control. For strongly long-range interactions ($\alpha$ small in the power-law $1/r^\alpha$), the light cone grows fast enough to undermine the savings.

## Related Paper Notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Trotter Commutator-Scaling Bound]]
