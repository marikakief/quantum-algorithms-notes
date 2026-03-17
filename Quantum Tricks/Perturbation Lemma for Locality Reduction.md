# Perturbation Lemma for Locality Reduction

> **Source:** Aharonov et al., arXiv:quant-ph/0405098, Lemma 3.17; also used in Kempe-Regev (2003)
> **Tags:** #trick #perturbation #locality #spectral-gap

## What it does

Lets you reduce the locality of a Hamiltonian (e.g., 5-local → 3-local) by simplifying terms and adding a large energy penalty $J$ for states that leave the legal subspace. The lemma bounds how much the low-energy spectrum shifts.

## Setup

$H = H_1 + H_2$ on Hilbert space $\mathcal{H} = S \oplus S^\perp$, where:
- $H_2$ has $S$ as a zero eigenspace and eigenvalues $\geq J$ on $S^\perp$
- $\|H_1\| \leq K$
- $J > 2K$

## The bounds

Let $a, b$ be the two lowest eigenvalues of $H_S$ (the restriction to $S$), and $a', b'$ the corresponding eigenvalues of the full $H$.

**Eigenvalue shift:**

$$
a - \frac{K^2}{J - 2K} \leq a' \leq a, \qquad b' \geq b - \frac{K^2}{J - 2K}
$$

**Ground state overlap:**

$$
|\langle \xi | \xi' \rangle|^2 \geq 1 - \frac{K^2}{(b - a)(J - 2K)}
$$

where $|\xi\rangle$, $|\xi'\rangle$ are the ground states of $H_S$ and $H$ respectively.

## How it's used

In the [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|adiabatic equivalence paper]]: the 5-local propagation terms $H_\ell$ (which use 3 clock qubits from the [[History-State Encoding with Unary Clock|unary clock]]) are replaced by 3-local terms $H'_\ell$ (using 1 clock qubit). The simplified terms can create illegal clock states, so $H_{\mathrm{clock}}$ is amplified by $J = \epsilon^{-2} L^6$.

With $K = O(L)$ (from $O(L)$ terms each of constant norm), the eigenvalue shift is $O(L^2 / L^6) = O(1/L^4)$, which is much smaller than the gap $\Omega(1/L^3)$.

## When to reach for it

- Reducing locality of Hamiltonian constructions (5-local → 3-local → 2-local)
- Any gadget construction where you simplify interactions and compensate with energy penalties
- QMA-completeness reductions

## Caveat

$J$ must scale as a high polynomial in $L$ and $1/\epsilon$ to keep the perturbation small relative to the gap. This feeds into the running time: $\|H\| \sim J \cdot L$, which inflates the adiabatic time bound. In the 3-local case, running time goes from $O(L^5)$ to $O(L^{14})$.

## Related notes

- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[History-State Encoding with Unary Clock]]
- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]
- [[Snake-Path 2D Embedding for Nearest-Neighbor Universality]]
