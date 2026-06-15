
> **Source:** Shor, arXiv:quant-ph/9508027 (1994), §4; Coppersmith (1994) for the approximate version
> **Tags:** #trick #QFT #circuit #fundamental

## What it does

Implements the quantum Fourier transform over $\mathbb{Z}_{2^n}$ — the unitary $|a\rangle \mapsto \frac{1}{\sqrt{2^n}} \sum_{c=0}^{2^n - 1} e^{2\pi i ac/2^n} |c\rangle$ — using $O(n^2)$ elementary gates (or $O(n \log n)$ for the approximate version).

## The circuit

Use two gate types:
- **$H_j$:** Hadamard on qubit $j$.
- **$CP_m$:** controlled phase $\operatorname{diag}(1,1,1,e^{2\pi i/2^m})$ between a pair of qubits.

A standard exact QFT applies, for each output qubit, one Hadamard and controlled phase rotations from the less-significant input bits that contribute to that output phase. Up to final bit reversal, the basis-state factorization is:

$$
F_{2^n}|a_1\ldots a_n\rangle
=
\bigotimes_{k=1}^{n}
\frac{|0\rangle+e^{2\pi i(0.a_{n-k+1}\ldots a_n)}|1\rangle}{\sqrt 2}.
$$

This product-state identity is the safest way to track the circuit: each output qubit encodes one binary fraction of the input bits. The usual circuit implements these binary fractions using controlled phase rotations.

**Output bit reversal:** many circuit diagrams output the qubits in reversed order. In algorithms where the register is immediately measured, this is usually handled by relabeling classical bits rather than physical SWAP gates.

## Approximate QFT (Coppersmith)

Drop controlled rotations below a threshold set by the target error. Coppersmith-style approximate QFT keeps only $O(\log(n/\varepsilon))$ nearby rotations per qubit to achieve overall error $O(\varepsilon)$, reducing the gate count to $O(n\log(n/\varepsilon))$ for period-finding-style applications.

## When to reach for it

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]: QFT is the core subroutine for order-finding
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Phase estimation]]: the inverse QFT extracts the phase bits
- Any algorithm that needs to convert between position and frequency representations
- Quantum simulation algorithms that use spectral analysis

## Complexity

| Version | Gates | All-to-all parallel depth | Line-nearest-neighbor depth |
|---|---:|---:|---:|
| Exact QFT | $O(n^2)$ | $O(n)$ with disjoint controlled rotations scheduled in layers | model/routing dependent, commonly $O(n^2)$ without extra structure |
| Approximate QFT | $O(n\log(n/\varepsilon))$ | still convention/model dependent; gate count improves first | model/routing dependent |
| Semiclassical inverse QFT | same controlled powers in the algorithm, fewer coherent qubits | measurement/feed-forward latency matters | architecture dependent |

## Caveat

The QFT is efficient as a quantum circuit, but the output is a quantum state — you can't read all $2^n$ amplitudes. The power comes from combining the QFT with structure in the input (periodicity, eigenvalue encoding) that concentrates the output amplitudes on a few basis states.

The controlled phase gates require interaction between potentially distant qubits. On architectures with limited connectivity, SWAP overhead can increase the depth.

Do not compare QFT depth without specifying connectivity, whether final bit reversal is physical, and whether a semiclassical inverse QFT is allowed.

## Related notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Coset Sampling via Fourier Transform]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] — optimistic QFT in $O(\log(n/\varepsilon))$ depth with zero ancillae and logarithmic-range 1D locality; uses block QPE to parallelise
- [[Quantum Phase Estimation on Blocks for QFT Parallelisation]] — the trick that enables the log-depth optimistic QFT
- [[Optimistic Quantum Circuits]] — framework formalising circuits with small average error
