# Replacement Process for State Reconstruction

> **Source:** Landau, Liu, arXiv:2410.23618
> **Tags:** #trick #state-learning #locality #shallow-circuits #tomography

## What it does

Rewrites part of a shallow state-preparation history using only local inversions and local resets, while leaving the target state unchanged.

## The trick

Suppose a local unitary $V_A$ disentangles region $A$ of the target state $|\psi\rangle$, so that

$$
V_A |\psi\rangle = |0\rangle_A \otimes |\phi\rangle.
$$

Define the replacement channel

$$
\Phi_A(\rho) = V_A^\dagger R_A(V_A \rho V_A^\dagger) V_A,
$$

where $R_A$ resets qubits in $A$ to $|0\rangle$. Then

$$
\Phi_A(|\psi\rangle\langle\psi|)=|\psi\rangle\langle\psi|.
$$

So you may insert an explicit reset to a known state into the preparation history without changing the target. That gives a handle for reconstructing the state from local pieces.

## When to reach for it

- You know or can learn local inversions of a structured state.
- You want to turn local disentangling data into an explicit preparation circuit.
- Direct stitching of local circuits would create a nasty consistency problem.

## Complexity

The channel itself is local. Overall cost depends on how hard it is to find the local inversion $V_A$ and how many replacement regions are needed.

## Caveat

This only works when the local inversion really exists and is accurate enough. For generic states, that promise is false.

## Related notes

- [[Learning quantum states prepared by shallow circuits in polynomial time (Landau-Liu 2024) — Paper Notes]]
- [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]]
- [[Sequential Disentanglement for MPS Tomography]]
