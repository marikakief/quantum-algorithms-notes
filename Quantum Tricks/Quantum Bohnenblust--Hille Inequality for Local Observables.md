# Quantum Bohnenblust--Hille Inequality for Local Observables

> **Source:** Huang, Chen, Preskill, arXiv:2210.14894
> **Tags:** #trick #local-hamiltonians #pauli-expansions #quantum-learning

## What it does

Controls a local observable by showing that a small operator norm forces its Pauli coefficients to be sparse in an $\ell_r$ sense with $r<2$.

## The trick

Let

$$
H = \sum_P \alpha_P P
$$

be a $k$-local Hamiltonian or observable written in the Pauli basis. Instead of bounding the coefficient vector by its $\ell_2$ norm, prove a stronger inequality of Bohnenblust--Hille type:

$$
\|H\| \gtrsim_k \|\alpha\|_{2k/(k+1)}.
$$

For bounded-degree observables this can be strengthened to an effective $\ell_1$ control. The paper gets this by first designing a better product-state optimization algorithm for local Hamiltonians, then turning that constructive approximation guarantee into a norm inequality.

The practical consequence is that a low-degree observable with bounded operator norm cannot hide too many large Pauli coefficients. That is exactly the sparsity statement needed for thresholded learning.

## When to reach for it

- You need to argue that a Pauli expansion is compressible.
- You want to bound shadow complexity for bounded-degree observables.
- You need a bridge from optimization over product states to coefficient sparsity.

## Complexity

Using the inequality is free once proved. The expensive part is whichever local-Hamiltonian optimization or coefficient-learning step it enables.

## Caveat

This is a structural inequality for local / bounded-degree observables. It does not rescue genuinely nonlocal operators whose Pauli mass is spread over high-weight terms.

## Related notes

- [[Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) — Paper Notes]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Thresholded Low-Degree Heisenberg Learning]]
