# Approximate-Eigenvector Phase Estimation for Noisy Modular Inner Products

> **Source:** Eldar--Hallgren, arXiv:2201.13450
> **Tags:** #trick #phase-estimation #approximate-eigenvectors #lattice-problems

## What it does

Extracts a modular phase from a state that is only an approximate eigenvector, paying an explicit precision penalty for eigenvector drift.

## The trick

Suppose

$$
\|U|\psi\rangle-\omega_q^s|\psi\rangle\|\leq \epsilon_{\mathrm{ev}}.
$$

For exact eigenvectors, phase estimation estimates $s/q$. For approximate eigenvectors, the problem is that controlled powers use $U^k|\psi\rangle$, and the error grows with $k$:

$$
\|U^k|\psi\rangle-\omega_q^{ks}|\psi\rangle\|\leq k\epsilon_{\mathrm{ev}}.
$$

Eldar--Hallgren compare the whole pre-measurement phase-estimation state with the ideal one. For power $T$, the state distance is at most $T\epsilon_{\mathrm{ev}}$ up to constants. Choosing $T$ as a function of the tolerated failure probability gives an output $O\in\mathbb{Z}_q$ satisfying

$$
\Pr\left[|O-s|_q\leq 129q\epsilon_{\mathrm{ev}}/p_{\mathrm{err}}^2\right]
\geq 1-p_{\mathrm{err}}.
$$

In the BDD application, the eigenphase is the character value

$$
\chi_a(-s)=\omega_q^{(-s)\cdot a},
$$

so phase estimation returns a noisy modular inner product

$$
O\approx (-s)\cdot a \pmod q.
$$

## When to reach for it

Use it when an operator is diagonal only approximately on a prepared state, but the approximate eigenvalue encodes useful algebraic data. Typical situations:

- spatial packets that shift almost into themselves;
- coset or lattice states with small displacement errors;
- phase labels from finite-abelian-group Fourier sampling;
- algorithms where coarse modular linear information is enough.

## Complexity

The phase-estimation runtime is polynomial in the register size and in the cost of applying the needed controlled shift powers, with precision limited by roughly

$$
q\epsilon_{\mathrm{ev}}/p_{\mathrm{err}}^2.
$$

In the Eldar--Hallgren lattice setting, $\epsilon_{\mathrm{ev}}=4n^{3/4}\sqrt{\epsilon_1}$ for a target within $\epsilon_1\lambda_1$ of the lattice.

## Caveat

This is not a way to get arbitrary precision from a rough eigenvector. The usable phase-estimation depth is capped by the drift: pushing $T$ too high makes $U^k|\psi\rangle$ depart from the ideal eigenstate and destroys the estimate.

## Related notes

- [[An Efficient Quantum Algorithm for Lattice Problems Achieving Subexponential Approximation Factor (Eldar-Hallgren 2022) — Paper Notes]]
- [[Phased Cube States as Approximate Shift Eigenvectors]]
- [[Coefficient-Preserving Random BDD Self-Reduction]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
