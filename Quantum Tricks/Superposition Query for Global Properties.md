# Superposition Query for Global Properties

> **Source:** Deutsch, Proc. R. Soc. Lond. A **400**, 97–117 (1985)
> **Tags:** #trick #query-complexity #superposition #fundamental

## What it does

Evaluates a function $f$ on a superposition of all inputs simultaneously, then uses interference to extract a **global property** of $f$ (constant vs balanced, periodicity, parity, etc.) from a single measurement — something that classically would require evaluating $f$ on multiple inputs.

## The idea

Classical query: evaluate $f(x)$ for one $x$ at a time. Each query reveals local information.

Quantum query: prepare $\sum_x \alpha_x |x\rangle$, apply $U_f$, and measure in a basis chosen to reveal global structure. The amplitudes for different $x$ values interfere, amplifying patterns and cancelling noise.

The global property must be extractable from the interference pattern — this constrains which properties benefit from quantum speedups.

## Instances

| Problem | Global property | Classical queries | Quantum queries |
|---|---|---|---|
| Deutsch | Constant vs balanced ($n=1$) | 2 | 1 |
| Deutsch-Jozsa | Constant vs balanced ($n$ bits) | $2^{n-1} + 1$ (deterministic) | 1 |
| Bernstein-Vazirani | Hidden string $s$ in $f(x) = s \cdot x$ | $n$ | 1 |
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes\|Simon]] | Hidden period $s$ ($f(x) = f(x \oplus s)$) | $\Omega(2^{n/2})$ | $O(n)$ |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes\|Shor]] | Period of $x^a \bmod N$ | $\Omega(N^{1/3})$ (number field sieve) | $O(\log^2 N)$ |
| [[Standard Amplitude Amplification\|Grover]] | Existence of marked item | $\Omega(N)$ | $O(\sqrt{N})$ |

## When to reach for it

- Designing new quantum algorithms: ask "what global property do I need, and can interference reveal it?"
- Understanding quantum speedups: the speedup comes from interference concentrating amplitude on the answer, not from "trying all inputs at once" (a common misconception)
- Teaching: this is the cleanest way to explain why quantum computers can be faster

## Caveat

Not all global properties benefit from quantum speedups. Unstructured search only gets a quadratic improvement ([[Standard Amplitude Amplification|Grover's $\sqrt{N}$]]). Exponential speedups require algebraic structure (periodicity, group actions) that the QFT or similar transforms can exploit.

The "evaluate all inputs at once" intuition is misleading — you can't read out all $f(x)$ values. The power is in choosing the right measurement basis to extract specific global information.

## Related notes

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]]
- [[Phase Kickback from Eigenstate Ancilla]]
- [[Coset Sampling via Fourier Transform]]
- [[Interference-Based Period Finding]]
- [[Standard Amplitude Amplification]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the original $O(\sqrt{N})$ search algorithm
- [[Inversion About the Mean]] — the key primitive that makes Grover search work
