
> **Source:** Deutsch, Proc. R. Soc. Lond. A **400**, 97–117 (1985)
> **Tags:** #trick #query-complexity #superposition #fundamental

## What it does

Applies one coherent oracle call to a superposition of inputs, then uses interference to extract a **global property** of $f$ (constant vs balanced, periodicity, parity, etc.) from a measurement in a carefully chosen basis. This is not access to all values of $f$; it is access to a specific interference pattern.

## The idea

Classical query: evaluate $f(x)$ for one $x$ at a time. Each query reveals local information.

Quantum query: prepare $\sum_x \alpha_x |x\rangle$, apply $U_f$, and measure in a basis chosen to reveal global structure. The amplitudes for different $x$ values interfere, amplifying patterns and cancelling noise.

The global property must be extractable from the interference pattern — this constrains which properties benefit from quantum speedups.

## Instances: query-model examples

| Problem | Model | Global property | Classical cost | Quantum cost |
|---|---|---|---|---|
| Deutsch | exact query | Constant vs balanced ($n=1$) | 2 queries | 1 query |
| Deutsch-Jozsa | exact vs deterministic query | Constant vs balanced ($n$ bits) | $2^{n-1} + 1$ deterministic queries | 1 query |
| Bernstein-Vazirani | query | Hidden string $s$ in $f(x)=s \cdot x$ | $n$ queries | 1 query |
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes\|Simon]] | bounded-error query/oracle | Hidden XOR mask $s$ | $\Omega(2^{n/2})$ queries | $O(n)$ queries |
| [[Standard Amplitude Amplification\|Grover]] | unstructured query | Existence of marked item | $\Omega(N)$ queries | $O(\sqrt{N})$ queries |

## Concrete runtime example

| Problem | Model | Classical cost | Quantum cost |
|---|---|---|---|
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes\|Shor factoring/order finding]] | bit/gate complexity, not a query lower bound | best known classical factoring is subexponential in $\log N$ (number field sieve) | polynomial in $\log N$ using modular exponentiation, QFT sampling, and continued fractions |

## When to reach for it

- Designing new quantum algorithms: ask "what global property do I need, and can interference reveal it?"
- Understanding quantum speedups: the speedup comes from interference concentrating amplitude on the answer, not from "trying all inputs at once" (a common misconception)
- Teaching: this is the cleanest way to explain why quantum computers can be faster

## Caveat

Not all global properties benefit from quantum speedups. Unstructured search only gets a quadratic improvement ([[Standard Amplitude Amplification|Grover's $\sqrt{N}$]]). Known standard exponential algorithmic speedups usually exploit strong structure such as periodicity, Fourier structure, or group actions; algebraic structure by itself is not sufficient.

The "evaluate all inputs at once" intuition is misleading — you can't read out all $f(x)$ values. The power is in choosing the right measurement basis to extract specific global information.

## Related notes

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]]
- [[Phase Kickback from Eigenstate Ancilla]]
- [[Coset Sampling via Fourier Transform]]
- [[Interference-Based Period Finding]]
- [[Standard Amplitude Amplification]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the original $O(\sqrt{N})$ search algorithm
- [[Inversion About the Mean]] — the key primitive that makes Grover search work
