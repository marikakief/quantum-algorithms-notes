# Quantum Fourier Transform Circuit

> **Source:** Shor, arXiv:quant-ph/9508027 (1994), §4; Coppersmith (1994) for the approximate version
> **Tags:** #trick #QFT #circuit #fundamental

## What it does

Implements the quantum Fourier transform over $\mathbb{Z}_{2^n}$ — the unitary $|a\rangle \mapsto \frac{1}{\sqrt{2^n}} \sum_{c=0}^{2^n - 1} e^{2\pi i ac/2^n} |c\rangle$ — using $O(n^2)$ elementary gates (or $O(n \log n)$ for the approximate version).

## The circuit

Two types of gates:
- **$R_j$:** Hadamard on qubit $j$ (creates $\frac{1}{\sqrt{2}}(|0\rangle + (-1)^{a_j}|1\rangle)$)
- **$S_{j,k}$:** Controlled phase gate on qubits $j < k$: $|1\rangle|1\rangle \mapsto e^{i\pi/2^{k-j}}|1\rangle|1\rangle$, identity on all other basis states

Apply in order (right to left on the state):

$$
R_{n-1},\; S_{n-2,n-1},\; R_{n-2},\; S_{n-3,n-1},\; S_{n-3,n-2},\; R_{n-3},\; \ldots,\; R_0
$$

Between $R_{j+1}$ and $R_j$: apply all $S_{j,k}$ for $k > j$.

**Output is bit-reversed** — either reverse qubit order with SWAPs ($O(n)$ gates) or read in reverse.

### Why it works

The total phase accumulated on the path $|a\rangle \to |b\rangle$ (with $b$ the bit-reversal of $c$) is:

$$
\sum_{j < l} \pi a_j b_j + \sum_{j < k < l} \frac{\pi}{2^{k-j}} a_j b_k = \frac{2\pi}{2^l} \left(\sum_j 2^j a_j\right)\left(\sum_k 2^k c_k\right) = \frac{2\pi ac}{q}
$$

using $b_k = c_{l-1-k}$ (bit reversal) and the distributive law. This matches the QFT phase $e^{2\pi iac/q}$.

## Approximate QFT (Coppersmith)

Drop all $S_{j,k}$ with $k - j > m$ for some threshold $m = O(\log n)$. The omitted phases $e^{i\pi/2^{k-j}}$ are exponentially small and contribute negligible error. This reduces the gate count to $O(n \log n)$ while maintaining sufficient precision for period finding.

## When to reach for it

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]: QFT is the core subroutine for order-finding
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Phase estimation]]: the inverse QFT extracts the phase bits
- Any algorithm that needs to convert between position and frequency representations
- Quantum simulation algorithms that use spectral analysis

## Complexity

| Version | Gates | Depth |
|---|---|---|
| Exact QFT | $O(n^2)$ | $O(n^2)$ |
| Approximate QFT | $O(n \log n)$ | $O(n \log n)$ |

## Caveat

The QFT is efficient as a quantum circuit, but the output is a quantum state — you can't read all $2^n$ amplitudes. The power comes from combining the QFT with structure in the input (periodicity, eigenvalue encoding) that concentrates the output amplitudes on a few basis states.

The controlled phase gates $S_{j,k}$ require interaction between potentially distant qubits. On architectures with limited connectivity, SWAP overhead can increase the depth.

## Related notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Coset Sampling via Fourier Transform]]
- [[Gapped Phase Estimation]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
