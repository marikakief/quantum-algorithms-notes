# MPS Overlap Extrapolation Protocol

> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748
> **Tags:** #trick #MPS #DMRG #overlap #state-preparation #FeMoco

## What it does

Estimates the overlap of a finite-bond-dimension MPS with an effectively exact state using only overlaps among lower-bond-dimension MPS approximations.

## The trick

Suppose direct access to $|\langle \Phi(M)|\Phi(\infty)\rangle|^2$ is impossible because the exact state is out of classical reach. Fit two empirical linear relations in $(\log M)^2$ instead:

$$
\log\!\bigl(1-|\langle \Phi(M')|\Phi(\infty)\rangle|^2\bigr)
\text{ vs. } (\log M')^2,
$$

and, for $M' \ll M''$,

$$
\log\!\bigl(|\langle \Phi(M')|\Phi(M'')\rangle|^2 - |\langle \Phi(M')|\Phi(\infty)\rangle|^2\bigr)
\text{ vs. } (\log M'')^2.
$$

Use lower-bond-dimension states $M'$ and somewhat larger reference states $M''$ to infer the unknown infinite-bond-dimension overlap, then extrapolate to the target $M$.

The point is not rigor. The point is to get a workable overlap estimate for QPE resource counting in systems where DMRG can still generate useful candidate states but not a certifiably exact ground state.

## When to reach for it

- DMRG/MPS-based QPE resource estimates for strongly correlated molecules.
- Cases where energy extrapolation is possible but overlap with the exact eigenstate is the quantity you actually need.
- Fe-S style problems with several competing low-energy states.

## Complexity

Classical post-processing only. The benefit is indirect: it lets you replace a vague overlap assumption with a fitted value, which then determines the QPE repetition cost.

## Caveat

This is empirical. It is not a theorem, and if the linear trends in $(\log M)^2$ break down, the estimate can drift badly. It is best used with validation on smaller instances from the same family.

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]]
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]]
