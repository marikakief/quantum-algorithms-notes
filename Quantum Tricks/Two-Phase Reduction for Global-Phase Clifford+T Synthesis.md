# Two-Phase Reduction for Global-Phase Clifford+T Synthesis

> **Source:** Ross and Selinger, arXiv:1403.2975
> **Tags:** #trick #gate-synthesis #Clifford-T #global-phase #z-rotations

## What it does

Reduces approximation up to arbitrary global phase to two ordinary Clifford+T synthesis searches.

## The trick

The up-to-phase problem asks for a Clifford+T operator $U$ and unit scalar $\lambda$ such that

$$
\lVert R_z(\theta)-\lambda U\rVert\le\varepsilon.
$$

Because Clifford+T determinants are discrete powers of $\omega=e^{i\pi/4}$, Ross--Selinger show that it suffices to test only

$$
\lambda\in\{1,e^{i\pi/8}\}.
$$

The $\lambda=1$ branch is the usual synthesis problem. For the $e^{i\pi/8}$ branch, use $\delta=1+\omega$, so that

$$
\frac{\delta}{|\delta|}=e^{i\pi/8}.
$$

Rewrite the matrix entries as $u'=\delta u$ and $t'=\delta t$. Then the same geometry reappears, with the candidate region scaled to

$$
u'\in |\delta|R_\varepsilon,
\qquad
(\nu')^\bullet\in |\delta^\bullet|D.
$$

Run the same grid enumeration and norm-equation completion, then compare the best even-T-count solution from the $\lambda=1$ branch with the best odd-T-count solution from the $e^{i\pi/8}$ branch.

## When to reach for it

- Gate synthesis problems where global phase is physically irrelevant but the exact gate set has discrete determinants.
- Any compilation method where considering all continuous phases would destroy a finite or countable candidate search.
- Clifford+T $R_z$ approximation when you want the actual minimum T-count rather than the minimum among determinant-one representatives.

## Complexity

The second branch has the same asymptotic cost as the first: expected $O(\operatorname{polylog}(1/\varepsilon))$ in Ross--Selinger. The grid normalisation can be shared because the regions differ only by fixed scalings.

## Caveat

The two-phase reduction depends on the determinant subgroup being discrete. It does not automatically apply to gate sets with continuous phases or to synthesis models where global phases can be introduced freely at zero cost outside the gate set.

## Related notes

- [[Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014) — Paper Notes]]
- [[Convex Grid Normalisation for Algebraic-Integer Candidate Search]]
- [[Norm-Equation Completion of Clifford+T Matrix Entries]]
- [[Clifford Hierarchy for Fault-Tolerant Gate Classification]]
