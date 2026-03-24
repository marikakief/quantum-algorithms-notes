# Variational Adiabatic Path Optimization

> **Source:** McClean, Romero, Babbush & Aspuru-Guzik, arXiv:1509.04279
> **Tags:** #trick #adiabatic #variational #VQE #scheduling

## What it does

Optimizes the adiabatic schedule path using only measurements at the endpoint, guided by the variational principle. No knowledge of the spectral gap is needed.

## The trick

In adiabatic state preparation with schedule $H(s) = A(s)H_i + B(s)H_p$, the final state is a functional of the path $f \in F(\tau)$. Parameterize the path by a small number of variables $\vec\theta$ (e.g., control points of a B-spline) and minimize:

$$\langle H_p \rangle(\vec\theta) = \langle \Psi[\vec\theta] | H_p | \Psi[\vec\theta] \rangle$$

using a classical optimizer (e.g., Nelder-Mead, COBYLA). The variational principle guarantees $\langle H_p \rangle(\vec\theta) \geq \lambda_1$, so this is a rigorous optimization.

The method naturally concentrates evolution time near gap bottlenecks: the optimizer discovers that slowing down near avoided crossings improves the endpoint energy, without ever measuring or computing the gap.

## Example

For Farhi et al.'s 1-qubit adiabatic problem with a gap controlled by $\epsilon = 0.1$: a 2-parameter B-spline path achieves the same fidelity as a linear schedule with $\sim 10\times$ longer evolution time. The variationally optimal path slows near the avoided crossing at $A(s) = 1/2$.

## When to reach for it

- Adiabatic state preparation on near-term hardware where evolution time $\tau$ is limited by decoherence
- When the spectral gap profile is unknown (black-box Hamiltonians)
- As an alternative to [[Gap-Adapted Adiabatic Scheduling]] when you can't compute a gap lower bound analytically
- Initializing [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|QAOA]] parameters from a continuous adiabatic path

## Complexity

Classical optimization cost: depends on the optimizer and number of path parameters. Each function evaluation requires running the full quantum evolution and measuring $\langle H_p \rangle$. With COBYLA or Powell, convergence in $O(d)$ to $O(d^2)$ function evaluations for $d$ parameters is typical.

Quantum cost per evaluation: same as a standard adiabatic evolution of duration $\tau$, plus measurement of $\langle H_p \rangle$ via [[Term-by-Term Expectation Estimation]].

## Caveat

The optimization is over a non-convex landscape. For simple parameterizations (few control points), the landscape is benign. For highly flexible parameterizations, local minima become a concern.

The method still requires coherent evolution for time $\tau$ — it reduces the $\tau$ needed for a given fidelity, but doesn't eliminate the coherence requirement.

## Related notes

- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]]
- [[Gap-Adapted Adiabatic Scheduling]]
- [[AQC-to-QAOA Parameter Transfer]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
