# Heisenberg Learning Tree Lower Bounds

> **Source:** Kannan, Putterman, and Cotler, arXiv:2605.02057
> **Tags:** #trick #lower-bounds #quantum-learning #heisenberg-picture #hypothesis-testing

## What it does

Proves adaptive noisy-learning lower bounds by replacing a whole learning tree with a nodewise Heisenberg-picture distinguishability calculation.

## The trick

A quantum learning algorithm can be represented as a tree: at each node $u$, the learner chooses a measurement or noisy quantum preprocessing based on all previous outcomes; each edge is labelled by an outcome probability. Directly bounding the total likelihood ratio of all possible adaptive paths is usually painful.

The Heisenberg learning tree method separates the job into three steps.

1. **Reduce to a one-vs-one tree.** For two hypotheses, compare the leaf distributions induced by states $\rho_0,\rho_1$.

2. **Push adaptivity into the Heisenberg picture.** At node $u$, absorb the learner's chosen operation into a Schrödinger-picture channel $\Phi_u$ acting on states and study

$$
\Delta^u=\Phi_u[\rho_0-\rho_1].
$$

Equivalently, for any node measurement effect $E_s$, push the channel onto the effect:

$$
\operatorname{tr}(E_s\Phi_u(\rho_b))
=
\operatorname{tr}(\Phi_u^\dagger(E_s)\rho_b).
$$

The proof no longer tracks every future adaptive branch. It bounds the distinguishability available at one node through these evolved effects / distinguishability operators.

3. **Convert a nodewise bound into a sample lower bound.** A martingale lemma controls the one-sided likelihood ratio along the tree. If for every node

$$
\mathbb E[(L(s\mid u)-1)^2]
$$

is small, then many samples are needed before the leaf distributions have total variation distance large enough for reliable hypothesis testing.

In the paper's third-moment lower bound, the fixed operator is built from permutation operators:

$$
\Delta_3=S_{123}-S_{12}S_3-S_{13}S_2-S_{23}S_1+2S_1^{\otimes 3}.
$$

Noisy depth-1 processing contracts this operator in the Pauli basis, so the lower bound reduces to computing a Hilbert--Schmidt norm such as

$$
\operatorname{tr}\bigl(N(\Delta_3)^2\bigr).
$$

## When to reach for it

- Adaptive quantum learning lower bounds where the scarce resource is **noise tolerance**, not copies.
- Multi-copy measurement lower bounds with constrained circuit depth.
- Situations where the hard distribution is naturally described by low moments or permutation operators.
- Problems where ordinary information-theoretic copy-count lower bounds are too blunt.

## Complexity

This is a proof method. Its cost is the algebra needed to evaluate the Heisenberg-evolved distinguishability operator. In good cases, that algebra factorises site-by-site, turning the lower bound into powers of small local trace calculations.

## Caveat

The method is only as clean as the operator you can isolate. For arbitrary noise channels or deep unconstrained circuits, the Heisenberg-evolved distinguishability operator may stop being tractable.

Also, this is a lower-bound tool, not an algorithm. It tells you why raw noisy learning fails; it does not build the uploaded protocol.

## Related notes

- [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]]
- [[Thresholded Low-Degree Heisenberg Learning]]
- [[Holevo Information Bottleneck for Learning Lower Bounds]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
