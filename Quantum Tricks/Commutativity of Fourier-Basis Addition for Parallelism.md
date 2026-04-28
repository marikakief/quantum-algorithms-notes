# Commutativity of Fourier-Basis Addition for Parallelism

> **Source:** Draper, arXiv:quant-ph/0008033
> **Tags:** #trick #quantum-arithmetic #parallelism #QFT #circuit-optimisation

## What it does
All controlled rotations in the QFT-based addition step commute with each other, allowing the entire addition to be executed in $O(\log n)$ parallel time slices (with AQFT truncation) instead of the sequential $O(n)$ depth of classical carry propagation.

## The trick

In the [[Arithmetic in the Fourier Basis|QFT adder]], the addition step applies controlled-$R_j$ rotations from the $b$-register qubits to the Fourier-basis $a$-register qubits. Each such rotation is a diagonal gate acting on exactly one $a$-qubit and one $b$-qubit. Diagonal gates on different qubits trivially commute. Even when two rotations share a qubit, they commute because they are both diagonal in the computational basis.

This is unlike the QFT circuit itself, where Hadamard gates (which change the basis) impose ordering constraints.

Because the rotations commute, they can be reordered freely. Group them by "depth": all $R_1$ rotations (one per Fourier qubit) in the first time slice, all $R_2$ rotations in the second, and so on. With AQFT truncation at $k = O(\log n)$, there are only $O(\log n)$ depth levels. If the hardware can execute $O(n)$ independent two-qubit gates simultaneously, the addition step completes in $O(\log n)$ time slices.

## When to reach for it

- Designing parallel quantum circuits for arithmetic-heavy algorithms (modular exponentiation for [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]], quantum simulation with Fourier-basis arithmetic).
- Architectures with high connectivity and parallel gate execution (e.g., trapped ions with all-to-all coupling).
- Any situation where you need fast addition and can tolerate the $O(n \log n)$ gate count of the approximate QFT adder.

## Complexity

| Metric | Sequential | Parallel ($n/2$ simultaneous gates) |
|---|---|---|
| Exact QFT adder depth | $O(n^2)$ | $O(n)$ |
| Approximate QFT adder depth | $O(n \log n)$ | $O(\log n)$ |

## Caveat

- The QFT and inverse QFT that bracket the addition do **not** have this commutativity property (Hadamards impose ordering). The parallel speedup applies only to the addition step itself.
- Hardware must support the required connectivity: each $b$-qubit must interact with each $a$-qubit. On nearest-neighbour architectures, swap overhead may negate the parallelism.

## Related notes
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — source paper
- [[Arithmetic in the Fourier Basis]] — the addition construction whose commutativity enables this trick
- [[Fourier State as Eigenstate of Addition]] — the underlying phase structure
