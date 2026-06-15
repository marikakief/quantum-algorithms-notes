# Irrep Extraction by Fourier-Conjugating the Regular Representation

> **Source:** Stephen P. Jordan, arXiv:0811.0562
> **Tags:** #trick #quantum-fourier-transform #representation-theory #finite-groups

## What it does

Turns an efficient group QFT into circuits for individual irreducible representation blocks.

## The trick

For a finite group $G$, implement the left-regular action
$$
U_g|h\rangle=|gh\rangle.
$$
Conjugate it by the quantum Fourier transform over $G$:
$$
U_{\mathrm{FT}}U_gU_{\mathrm{FT}}^{-1}
= \bigoplus_{\rho\in\widehat G}\rho(g^{-1})\otimes I_{d_\rho}.
$$
The Fourier label $\rho$ selects the irrep, one matrix index is acted on by $\rho(g^{-1})$, and the other is the multiplicity index from the regular representation.

To estimate a matrix element, prepare the Fourier-basis state $|\rho,i,j\rangle$, run the conjugated regular action, and use a Hadamard test or a polarization identity for off-diagonal entries.

## When to reach for it

- A finite group has an efficient quantum Fourier transform and efficient reversible multiplication.
- You want matrix elements of irreps in the QFT's subgroup-adapted basis.
- You need a clean way to use nonabelian Fourier machinery outside an HSP sampling setting.

## Complexity

If the QFT and inverse QFT over $G$ cost $T_{\mathrm{QFT}}$ gates and group multiplication by $g$ costs $T_{\mathrm{mult}}$, one represented group element costs
$$
O(T_{\mathrm{QFT}}+T_{\mathrm{mult}}).
$$
A Hadamard-test estimate to additive error $\epsilon$ uses $O(1/\epsilon^2)$ repetitions.

## Caveat

The basis is whatever the QFT gives you. For nonabelian groups, basis choice matters; this trick does not automatically produce the basis most convenient for a downstream representation-theory problem. It also gives additive estimates, which may be uninformative for typical exponentially small matrix elements.

## Related notes

- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]
