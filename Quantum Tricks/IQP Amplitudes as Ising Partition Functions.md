# IQP Amplitudes as Ising Partition Functions

> **Source:** Mann-Bremner, arXiv:1806.11282; Bremner-Jozsa-Shepherd (2011)
> **Tags:** #trick #IQP #Ising-model #quantum-supremacy #classical-simulation

## What it does
Expresses the output amplitude of an IQP (Instantaneous Quantum Polynomial-time) circuit as a scaled Ising partition function with imaginary parameters, connecting quantum circuit simulation to classical combinatorial counting.

## The trick
An IQP circuit is a Hamiltonian $H_{X_G} = -\sum_{\{u,v\}} \omega_{\{u,v\}} X_u X_v - \sum_v \upsilon_v X_v$ diagonal in the Pauli-$X$ basis, applied to $|0^n\rangle$. The all-zeros output amplitude is:

$$\psi_G = \langle 0^n | e^{-iH_{X_G}} | 0^n \rangle = \frac{1}{2^{|V|}} Z_{\text{Ising}}(G;\, i\Omega,\, i\Upsilon)$$

where $Z_{\text{Ising}}$ is evaluated at **imaginary** interactions and fields.

This means any classical algorithm for $Z_{\text{Ising}}$ with complex parameters immediately gives classical simulation of IQP amplitudes. Conversely, the #P-hardness of $Z_{\text{Ising}}$ for general complex parameters implies classical hardness of simulating IQP circuits.

## When to reach for it
- Mapping quantum circuit simulation questions to classical counting problems
- Understanding the easy/hard boundary for classical simulation of commuting circuits
- When you want to leverage statistical mechanics tools (Lee-Yang zeros, cluster expansions) for quantum complexity questions

## Complexity
When $|\omega_e|, |\upsilon_v| = O(1/\Delta)$, the amplitude is classically approximable in polynomial time. For general angles, it's #P-hard.

## Caveat
Only applies to commuting circuits (IQP). Non-commuting circuits don't have a direct Ising model interpretation, though they can be expressed via more complex partition functions (Tutte polynomial, tensor networks, etc.).

## Related notes
- [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes]]
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]]
- [[Barvinok Interpolation for Partition Function Approximation]]
