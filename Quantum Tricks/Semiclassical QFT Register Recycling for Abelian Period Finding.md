# Semiclassical QFT Register Recycling for Abelian Period Finding

> **Source:** Griffiths--Niu, arXiv:quant-ph/9511007; Proos--Zalka, arXiv:quant-ph/0301141 for the ECDLP application
> **Tags:** #trick #QFT #phase-estimation #space-saving #Shor

## What it does

Replaces a Fourier/control register by one measured control qubit reused across the controlled powers or shifts, when that register would only be inverse-QFTed and immediately measured.

## The trick

In period-finding and discrete-log algorithms, the input register starts as a product of $|+\rangle$ qubits, controls a sequence of known unitaries, and is measured after an inverse [[Quantum Fourier Transform Circuit]]. The Griffiths--Niu observation is that a QFT followed immediately by computational-basis measurement can be implemented semiclassically: measure qubits one at a time, adapting each measurement basis from earlier outcomes.

The replacement is valid when the register starts in a product superposition, controls known unitaries, and is not needed coherently after Fourier readout. For ECDLP, instead of storing

$$
\frac{1}{N}\sum_{x,y=0}^{N-1}|x,y\rangle|0\rangle
$$

and then computing $xP+yQ$, process bits of $x$ and $y$ sequentially. With the convention used by Proos--Zalka this is high to low significance; other inverse-QFT conventions reverse the order:

1. prepare one qubit in $|+\rangle$;
2. conditionally apply a fixed group shift by $2^iP$ or $2^iQ$;
3. measure that qubit in the current semiclassical Fourier basis;
4. update the classical feed-forward phase and reuse the qubit.

The measured bit is exactly the bit that the QFT output register would have produced, so the sampling distribution is unchanged in the ideal circuit.

## When to reach for it

- Shor-style algorithms where the Fourier register only controls powers/shifts and is then measured.
- Phase-estimation circuits where coherent access to the phase bits is not needed later.
- Resource estimates where qubits matter more than modest classical feed-forward complexity.

## Complexity

Saves the full Fourier input registers: for ECDLP, the two $n$-qubit registers for $x$ and $y$ become one reusable control qubit plus classical measurement memory. The algorithmic gate count is essentially unchanged, but adaptive single-qubit rotations, measurement latency, and classical feed-forward become part of the physical schedule.

## Caveat

This only works when the QFT output is measured immediately. If later coherent computation depends on the Fourier bits, one cannot replace the register by classical outcomes without changing the algorithm.

## Related notes

- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]]
- [[Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- Beauregard-style small-qubit factoring circuits use the same semiclassical-QFT register-recycling idea
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]]
- [[Quantum Fourier Transform Circuit]]
