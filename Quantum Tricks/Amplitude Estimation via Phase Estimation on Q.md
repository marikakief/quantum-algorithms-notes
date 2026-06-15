
> **Source:** Brassard, Høyer, Mosca & Tapp, arXiv:quant-ph/0005055 (2002), Section 4
> **Tags:** #trick #amplitude-estimation #phase-estimation #counting

## What it does

Estimates the success probability $a$ of a quantum algorithm $A$ by applying [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] to the Grover operator $Q = -AS_0 A^{-1}S_\chi$. The eigenphases of $Q$ are $\pm 2\theta_a$ where $a = \sin^2\theta_a$, so extracting $\theta_a$ gives $a$.

## The trick

1. The operator $Q$ from [[Standard Amplitude Amplification|amplitude amplification]] has eigenvalues $e^{\pm 2i\theta_a}$ on the relevant 2D subspace
2. Apply standard QPE to $Q$, using controlled powers of $Q$.
3. The measured phase $\tilde{\theta}_a$ gives $\tilde{a} = \sin^2\tilde{\theta}_a$

**Precision (Theorem 12):** With $M$ applications of $Q$:

$$
|\tilde{a} - a| \leq \frac{2\pi\sqrt{a(1-a)}}{M} + \frac{\pi^2}{M^2}
$$

with probability $\geq 8/\pi^2$.

## Application: approximate counting

For counting $t$ solutions among $N$ elements, use $A$ = Hadamard transform, $\chi$ = the function $f$. Then $a = t/N$ and estimating $a$ to additive error $\epsilon$ costs $O(1/\epsilon)$ applications of $Q$, each costing $O(1)$ evaluations of $f$. Thus estimating $t$ within additive error $\epsilon N$ costs $O(1/\epsilon)$ oracle evaluations, apart from state-preparation/reflection costs.

| Counting goal | Query scale |
|---|---|
| Additive error $\epsilon$ in $a=t/N$ | $O(1/\epsilon)$ |
| Additive error $\eta$ in $t$ | $O(N/\eta)$ |
| Relative error $\epsilon t$ for $t>0$ | $O(\sqrt{N/t}/\epsilon)$ using the counting/relative-estimation procedure |
| BHT/BHMT phase-register parameter $P$ | Error $O(\sqrt{tN}/P + N/P^2)$ in $t$ |

## When to reach for it

- Counting solutions to a search problem
- Estimating acceptance probability of a quantum circuit
- Subroutine in [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]]: estimate norm $\|A^{-1}|b\rangle\|$
- Any setting where you need $\langle\psi|P|\psi\rangle$ for a projector $P$, provided the reflection/check $S_\chi$ for that projector can be implemented coherently

## Complexity

$O(1/\epsilon)$ applications of $Q$ for additive error $\epsilon$ in $a$. This is quadratically better than classical sampling ($O(1/\epsilon^2)$).

## Caveat

Requires controlled powers of $Q$, which means controlled versions of $A$, $A^{-1}$, and $S_\chi$. If controlling $A$ or implementing the projector reflection is expensive, the overhead can dominate. [[Kaiser-Window Amplitude Estimation|Kaiser-window]] and related variants change the spectral window and constants; they do not remove the need for coherent access in the standard oracle model.

## Related notes

- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) — Paper Notes]] — applies AE to estimate partition function ratios via quantum walk stationary state preparation
- [[Standard Amplitude Amplification]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Kaiser-Window Amplitude Estimation]]
