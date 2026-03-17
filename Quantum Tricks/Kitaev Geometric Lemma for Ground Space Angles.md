# Kitaev Geometric Lemma for Ground Space Angles

> **Source:** Kitaev, Shen & Vyalyi, *Classical and Quantum Computation* (2002), Lemma 14.4
> **Tags:** #trick #spectral-gap #QMA #perturbation

## What it does

Lower bounds the ground energy of a sum of two positive semidefinite Hamiltonians using the angle between their ground spaces. If the ground spaces are "far apart," the sum has higher ground energy.

## The lemma

Let $H_1, H_2$ be positive semidefinite with ground energies $a_1, a_2$ and spectral gaps $\geq \Lambda$ above their ground spaces. Let $\theta$ be the angle between the two ground spaces. Then:

$$
\lambda_{\min}(H_1 + H_2) \geq a_1 + a_2 + 2\Lambda \sin^2(\theta/2)
$$

## When to reach for it

- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Adiabatic equivalence proofs]]: bounding the gap between $S_0$ (correct input) and $S_j$ (wrong input) sectors
- QMA-completeness reductions: showing that wrong witnesses have high energy
- Any setting where you have two penalty Hamiltonians and want to show their ground spaces don't overlap much

## Caveat

Requires knowing the spectral gap $\Lambda$ and the angle $\theta$. The angle is often bounded using [[Monotonicity Bootstrap for Conductance Bounds|monotonicity]] of the ground state components rather than computed directly.

## Related notes

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Perturbation Lemma for Locality Reduction]]
- [[Monotonicity Bootstrap for Conductance Bounds]]
