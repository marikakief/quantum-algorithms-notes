# Phased Cube States as Approximate Shift Eigenvectors

> **Source:** Eldar--Hallgren, arXiv:2201.13450
> **Tags:** #trick #lattice-problems #phase-estimation #finite-abelian-groups #bounded-distance-decoding

## What it does

Turns a finite-lattice superposition with an otherwise awkward Fourier phase into an approximate eigenvector for shifts close to lattice points.

## The trick

Let $G\leq\mathbb{Z}_q^n$ be the finite quotient of a $q$-periodic lattice, with coefficient space $\widetilde C$ and generators $G c$. For side length $2\sigma$, define

$$
|C(y)\rangle=\frac{1}{(2\sigma)^{n/2}}\sum_{z\in[\pm\sigma]^n}|y+z\rangle.
$$

Instead of trying to erase the coefficient register in

$$
\sum_{c\in\widetilde C}|c\rangle|C(Gc)\rangle,
$$

Fourier-transform and measure it. This leaves a random phase label $a$ and the state

$$
|\psi_a\rangle=\frac{1}{\sqrt{|G|}}\sum_{c\in\widetilde C}\chi_a(c)|C(Gc)\rangle.
$$

For a true group shift $v=Gc$, the shift operator $U_v|y\rangle=|y+v\rangle$ acts diagonally:

$$
U_v|\psi_a\rangle=\chi_a(-c)|\psi_a\rangle.
$$

For a near-group shift $y=Gs+\Delta$, the cube overlap gives

$$
\|U_y|\psi_a\rangle-\chi_a(-s)|\psi_a\rangle\|
\leq 4n^{3/4}\sqrt{\frac{\|\Delta\|_q}{\lambda_1(G)}}
$$

when the cube side length lies in

$$
\frac14\frac{\lambda_1(G)}{\sqrt n}\leq 2\sigma\leq \frac12\frac{\lambda_1(G)}{\sqrt n}.
$$

The phase is not a nuisance; it is the eigenvalue that carries the hidden coefficient information.

## When to reach for it

Use this when:

- a group/lattice state has a coefficient register that is hard to uncompute;
- shifting by a hidden structured element is easy to implement;
- small perturbations of the shift should only slightly disturb a spatial packet;
- the desired information can be expressed as a character value $\chi_a(s)$.

## Complexity

State preparation costs $\operatorname{poly}(n,\log q)$ for the finite-group arithmetic and Fourier transforms. The approximation error scales like

$$
O\left(n^{3/4}\sqrt{\frac{\|\Delta\|_q}{\lambda_1(G)}}\right),
$$

which later limits phase-estimation precision.

## Caveat

The cube geometry costs factors of $n$ and the eigenvector property degrades under repeated powers of the shift. If the target is not sufficiently close to $G$, phase estimation sees too much drift to recover useful information.

## Related notes

- [[An Efficient Quantum Algorithm for Lattice Problems Achieving Subexponential Approximation Factor (Eldar-Hallgren 2022) — Paper Notes]]
- [[Approximate-Eigenvector Phase Estimation for Noisy Modular Inner Products]]
- [[Coefficient-Preserving Random BDD Self-Reduction]]
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
