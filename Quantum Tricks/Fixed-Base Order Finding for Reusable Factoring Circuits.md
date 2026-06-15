# Fixed-Base Order Finding for Reusable Factoring Circuits

> **Source:** Grosshans-Lawson-Morain-Smith, arXiv:1511.04385
> **Tags:** #trick #factoring #order-finding #circuits

## What it does

For the safe-semiprime promise studied by Grosshans--Lawson--Morain--Smith, fixes the order-finding base independently of the unknown factors so repeated attempts reuse the same modular-exponentiation circuit.

## The trick

Standard [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]] chooses a random base $a$ for each attempt, then runs order finding for the unitary

$$
|x\rangle\mapsto |a x \bmod N\rangle.
$$

Changing $a$ changes the modular multiplication constants and hence changes the compiled order-finding circuit.

For safe semiprimes $N=p_1p_2$ with $p_i=2q_i+1$ and odd primes $q_i$, Grosshans--Lawson--Morain--Smith instead choose the fixed base

$$
a=2.
$$

This choice does not use the factors of $N$ and is valid only when $2$ is coprime to $N$ and the paper's safe-semiprime exclusions hold. In that promise case it has two useful properties:

1. By the paper's number-theory lemma, $\operatorname{ord}_N(2)$ is forced to be $q_1q_2$ or $2q_1q_2$.
2. Multiplication by $2$ modulo $N$ is circuit-friendly compared with a generic modular multiplier.

If the QOFA sample is one of the rare uninformative outputs, run the same circuit again. The next sample has the same success probability and requires no new base selection or recompilation.

## When to reach for it

- Experimental or resource-estimation settings where recompiling a modular exponentiation circuit is expensive.
- Structured factoring tasks where a small fixed base has provable order constraints.
- Any algorithm where the quantum subroutine is costly but multiple independent samples from the same unitary are acceptable.

## Complexity

The asymptotic order-finding cost per run is unchanged: one modular exponentiation plus a QFT/continued-fraction extraction as in [[Order-Finding via QFT and Continued Fractions]]. The saving is in circuit reuse and, under the safe-semiprime theorem, in making the classical post-processing failure probability tiny. In the idealized count this gives expected runs

$$
\frac{1}{1-1/(q_1q_2)}=1+O(1/N).
$$

## Caveat

A fixed base is dangerous if it was chosen using hidden knowledge of the factors; that gives misleading “compiled Shor” demonstrations. The base must be factor-independent, and its order properties must be proved from the input promise, not from the solution.

For general composites, Shor's random-base order finding remains the standard algorithm. This fixed-base trick is promise-specific, not a generic recommendation to use base $2$ for arbitrary RSA moduli.

## Related notes

- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]
- [[Safe-Semiprime Factoring from a QOFA Divisor]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Reversible Modular Exponentiation with Garbage Cleanup]]
