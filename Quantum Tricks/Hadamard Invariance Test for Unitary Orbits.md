# Hadamard Invariance Test for Unitary Orbits

> **Source:** Harrow--Lin--Montanaro, arXiv:1607.03236
> **Tags:** #trick #Hadamard-test #property-testing #unitary-orbits #isomorphism

## What it does

Tests whether a state is fixed by one of many known unitaries, using tensor powers to suppress false positives.

## The trick

For a unitary $U_i$ and state $|\psi\rangle$, apply a controlled-$U_i$ Hadamard test to each of $k$ copies of

$$
\frac1{\sqrt2}(|0\rangle+|1\rangle)|\psi\rangle.
$$

Accept only if all $k$ Hadamard outputs are $0$. The acceptance probability is

$$
\left(\frac12+\frac12\operatorname{Re}\langle\psi|U_i|\psi\rangle\right)^k.
$$

If $U_i|\psi\rangle=|\psi\rangle$, this is 1. If

$$
|\langle\psi|U_i|\psi\rangle|\le1-\epsilon,
$$

then it is at most $e^{-\epsilon k/2}$. Setting $k=O((\log n)/\epsilon)$ makes the false-positive probability $O(1/n)$, and the average-measurement OR test checks all $i$.

## When to reach for it

- Property testing under group actions.
- Orbit membership / invariance questions.
- Query-efficient isomorphism testers where the group is large but $\log |G|$ is manageable.

## Caveat

The trick gives query efficiency. Time efficiency depends on implementing the controlled unitaries and the OR over them.

## Related notes

- [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]]
- [[Function Isomorphism Testing via Orbit States]]
- [[Hadamard Test]]
