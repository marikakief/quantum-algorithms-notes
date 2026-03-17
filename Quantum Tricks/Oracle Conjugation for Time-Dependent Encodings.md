# Oracle Conjugation for Time-Dependent Encodings

> **Source:** Guang Hao Low and Nathan Wiebe, *Hamiltonian Simulation in the Interaction Picture*, arXiv:1805.00675  
> **Tags:** #trick #oracle-design #interaction-picture #dyson

## Idea

Given a [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] oracle $O_B$ for the static Hamiltonian $B$, construct a time-dependent oracle for the [[Interaction-Picture Norm Reduction|interaction-picture]] Hamiltonian $H_I(t) = e^{iAt}Be^{-iAt}$ by conjugation:

$$
O_{H_I}(t) = e^{iAt} \cdot O_B \cdot e^{-iAt}.
$$

This is the implementation-level step that turns the [[Interaction-Picture Norm Reduction|interaction-picture]] idea into a circuit. Without it, the Dyson algorithm has no way to query $H_I(t)$ at different times.

## How it's used in 1805.00675

The truncated-Dyson algorithm (from Kieferová–Scherer–Berry) needs a controlled-time oracle: for each sampled time $\tau_j$ in the Dyson expansion, apply $H_I(\tau_j)$ to the system. This is done by:

1. [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] the time register in superposition over $\tau_1 \le \cdots \le \tau_k$.
2. For each $\tau_j$ in the superposition, apply $e^{-iA\tau_j}$, then $O_B$, then $e^{iA\tau_j}$.

Step 2 costs one query to $O_B$ plus two applications of $e^{\pm iAt}$ per time sample. This is where the requirement that $e^{iAt}$ is cheap enters the gate count.

## When to use

Use this whenever you're working in an [[Interaction-Picture Norm Reduction|interaction picture]] and need to provide a Hamiltonian oracle for the [[Interaction-Picture Norm Reduction|interaction picture]] term. The conjugation structure is standard; the cost depends entirely on how expensive $e^{\pm iAt}$ is.

## Caveat

If $A$ has fast-forwardable dynamics (diagonal, free fermions, etc.), the conjugation is cheap. Otherwise this step can dominate the query budget. The gain from working in the [[Interaction-Picture Norm Reduction|interaction picture]] is contingent on this cost being small.

## Related notes

- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Interaction-Picture Norm Reduction]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
