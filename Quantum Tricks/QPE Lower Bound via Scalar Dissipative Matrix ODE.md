# QPE Lower Bound via Scalar Dissipative Matrix ODE

> **Source:** Simon, Berry, and Somma, arXiv:2605.16195
> **Tags:** #trick #lower-bound #phase-estimation #differential-equations

## What it does

Proves a matrix-ODE entry-estimation lower bound by hiding a phase-estimation discrimination problem in a single scalar dissipative entry.

## The trick

Take the restricted matrix ODE instance

$$
\dot X(t)=A^\dagger X(t)+C,
\qquad C=I,
\qquad B=D=0,
$$

with diagonal dissipative

$$
A=-\sin\theta\,|0\rangle\!\langle 0|-
\sum_{n>0}|n\rangle\!\langle n|.
$$

Then the entry of interest is a scalar integral:

$$
\langle 0|X(t)|0\rangle
=
\frac{1-e^{-2t\sin\theta}}{2\sin\theta}.
$$

Distinguishing $\theta=\delta$ from $\theta\geq 2\delta$ requires resolving this entry to accuracy proportional to the gap between the two scalar integrals.

The block-encoding of $A$ can be built from the phase oracle whose eigenphase is $\theta$, so an algorithm that estimates the ODE entry too quickly would solve a decision version of [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|quantum phase estimation]] too quickly.

## When to reach for it

Use this pattern when a proposed quantum differential-equation algorithm claims better-than-Heisenberg dependence on time or precision. If a scalar dissipative instance can encode phase discrimination, phase-estimation lower bounds usually come back.

## Complexity

Simon--Berry--Somma prove that, for $t\geq 6$ and $\epsilon\leq t/100$, estimating $\langle 0|X(t)|0\rangle$ with success probability $p\in(3/4,1)$ requires

$$
\Omega\!\left(\frac{Lt}{\epsilon}|\log(1-p)|\right)
$$

queries, matching their upper bound up to logarithmic factors.

## Caveat

This is an oracle lower bound for the block-encoding access model. It does not rule out improvements from extra promise structure, different input models, or a weaker output task.

## Related notes

- [[Efficient Quantum Algorithm for Linear Matrix Differential Equations and Applications to Open Quantum Systems (Simon-Berry-Somma 2026) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
