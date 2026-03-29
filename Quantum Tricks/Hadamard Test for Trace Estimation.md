# Hadamard Test for Trace Estimation

> **Source:** Aharonov, Jones & Landau, arXiv:quant-ph/0511096 (standard technique, used prominently here)
> **Tags:** #trick #estimation #trace #matrix-element

## What it does

Estimates $\text{Re}\langle\alpha|U|\alpha\rangle$ (or $\text{Im}\langle\alpha|U|\alpha\rangle$) for a unitary $U$ and an efficiently preparable state $|\alpha\rangle$, using a single ancilla qubit.

## The trick

1. Prepare $\frac{1}{\sqrt{2}}(|0\rangle + |1\rangle) \otimes |\alpha\rangle$.
2. Apply controlled-$U$: $|0\rangle|\alpha\rangle + |1\rangle U|\alpha\rangle$ (unnormalised).
3. Apply Hadamard to the control qubit.
4. Measure the control. Output $+1$ if $|0\rangle$, $-1$ if $|1\rangle$.

The expectation of the output is $\text{Re}\langle\alpha|U|\alpha\rangle$.

For the imaginary part, start with $\frac{1}{\sqrt{2}}(|0\rangle - i|1\rangle) \otimes |\alpha\rangle$ instead.

Repeat $O(1/\varepsilon^2)$ times and average for additive error $\varepsilon$.

## When to reach for it

- Estimating diagonal matrix elements $\langle\psi|U|\psi\rangle$ of unitaries.
- Computing traces: $\text{tr}(U)/d$ is the expectation over random $|\alpha\rangle$.
- Jones polynomial / Tutte polynomial algorithms (where the answer is a matrix element of a braid unitary).
- Any setting where you need to estimate $\langle\alpha|U|\alpha\rangle$ without full quantum state tomography.

## Complexity

$O(1/\varepsilon^2)$ repetitions for additive error $\varepsilon$. Each repetition uses one application of controlled-$U$ and preparation of $|\alpha\rangle$.

## Caveat

Only gives additive error, not multiplicative. If $\langle\alpha|U|\alpha\rangle$ is exponentially small, the estimate is swamped by noise. This is precisely the additive-vs-multiplicative approximation issue that plagues the Jones polynomial algorithms.

The controlled-$U$ may double the circuit depth relative to $U$ itself.

## Related notes
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
