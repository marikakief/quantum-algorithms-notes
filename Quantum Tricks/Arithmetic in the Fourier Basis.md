# Arithmetic in the Fourier Basis

> **Source:** Draper, arXiv:quant-ph/0008033
> **Tags:** #trick #quantum-arithmetic #QFT #adder #circuit-primitive

## What it does
Adds two $n$-bit integers without carry qubits by working in the [[Quantum Fourier Transform Circuit|QFT]] basis, where addition reduces to commuting phase rotations.

## The trick

The [[Quantum Fourier Transform Circuit|QFT]] maps $|a\rangle$ to a product state $|\phi_n(a)\rangle \otimes \cdots \otimes |\phi_1(a)\rangle$, where each qubit encodes $k$ low-order bits of $a$ in its phase:

$$|\phi_k(a)\rangle = \frac{1}{\sqrt{2}}\left(|0\rangle + e^{2\pi i \cdot 0.a_k a_{k-1} \cdots a_1}|1\rangle\right)$$

To add $b$ to this representation, apply controlled-$R_j$ rotations (each a diagonal phase gate $\text{diag}(1,1,1,e^{2\pi i/2^j})$) conditioned on the bits of $b$:

$$|\phi_k(a)\rangle \xrightarrow{R_1(b_k), R_2(b_{k-1}), \ldots} |\phi_k(a+b)\rangle$$

The full procedure is: QFT $\to$ phase rotations $\to$ inverse QFT.

No carry bits are allocated or propagated. The "carry" information is encoded in the phases. The cost is $\frac{1}{2}n(n+1)$ controlled rotations for the addition step (plus $O(n^2)$ for the forward and inverse QFT). Using the approximate QFT (truncating rotations at $k = O(\log n)$), the total drops to $O(n \log n)$.

## When to reach for it

- When qubit count is the bottleneck and you can tolerate $O(n^2)$ or $O(n \log n)$ abstract rotation count. Depth depends on the QFT implementation, rotation scheduling, connectivity, and any fault-tolerant synthesis strategy.
- Adding a classical constant to a quantum register: the $b$-register becomes compile-time angles, and you need only $n$ qubits total.
- When subsequent computation can stay in the Fourier basis, amortising the QFT cost across multiple operations.
- Inside [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] for minimal-qubit implementations ($2n$ qubits instead of $3n$).

## Complexity

| Variant/stage | Abstract gates | Depth note | Qubits |
|---|---|---|---|
| Addition rotations only, exact | $O(n^2)$ controlled rotations | rotations commute; scheduling depends on parallelism/connectivity | $2n$ |
| Addition rotations only, AQFT-truncated | $O(n \log n)$ controlled rotations | can be $O(\log n)$ layers with enough parallelism and compatible connectivity | $2n$ |
| Full QFT-add-inverse-QFT adder | three $O(n^2)$ stages, or three $O(n \log n)$ AQFT-truncated stages | includes forward/inverse QFT depth; model dependent | $2n$ |
| Classical-to-quantum, exact | $O(n^2)$ unconditional rotations plus QFTs | offset register is compiled into angles | $n$ |

## Caveat

- The $O(n^2)$ gate count is worse than $O(n)$ for classical reversible adders. This is a qubit/depth tradeoff.
- On fault-tolerant hardware, each controlled rotation must be synthesized or implemented by a rotation gadget. The abstract $O(n \log n)$ AQFT count is not automatically a low T-count implementation, and is usually worse in T resources than Toffoli-based adders like [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]].
- The approximate version introduces truncation error. For most applications this is negligible, but it must be tracked in an error budget.

## Related notes
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — source paper
- [[Fourier State as Eigenstate of Addition]] — the phase kickback property that makes this work
- [[Commutativity of Fourier-Basis Addition for Parallelism]] — parallelisation of the addition step
- [[Phase Gradient Rotation Synthesis]] — uses addition into a Fourier state for rotation synthesis
- [[Phase Gradient State for Controlled Rotations]] — the resource state version of this idea
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — fault-tolerant adder optimised for T-count instead
