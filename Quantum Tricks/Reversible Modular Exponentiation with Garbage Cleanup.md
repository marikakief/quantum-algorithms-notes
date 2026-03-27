
> **Source:** Shor, arXiv:quant-ph/9508027 (1994), §3; using Bennett (1973) reversible computation
> **Tags:** #trick #reversible-computation #modular-arithmetic #factoring

## What it does

Computes $|a\rangle|1\rangle \mapsto |a\rangle|x^a \bmod N\rangle$ reversibly, cleaning up all garbage bits. This is the bottleneck subroutine in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's factoring algorithm]].

## The construction

**Forward computation (repeated squaring):**

```
power := 1
for i = 0 to l-1:
    if a_i == 1:
        power := power * x^{2^i} (mod N)
```

The values $x^{2^i} \bmod N$ are computed classically and hardwired into the circuit. Only $a$ is quantum input; $x$ and $N$ are constants.

**Garbage cleanup via modular inverse:**

The forward computation produces $(a, b, x^a \bmod N)$ where $b$ is garbage. To erase $b$:

1. Copy $\mathrm{result} = x^a \bmod N$ to a fresh register
2. Compute $c^{-1} \bmod N$ (classically, since $\gcd(x^{2^i}, N) = 1$)
3. Run the inverse circuit: multiply by $c^{-1}$ controlled on result bits, undoing the forward computation
4. Garbage register returns to 0

This works because multiplication by $c$ is a bijection $\bmod N$ (since $\gcd(c, N) = 1$), so its inverse is well-defined.

**Shor's quantum watchdog:** After cleanup, measure the garbage register. If it's not $|0\rangle$, an error occurred — restart. If it is $|0\rangle$, the measurement projects out error components, improving reliability. This is an early form of mid-circuit error detection.

## When to reach for it

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]: the modular exponentiation is the most expensive step
- Any quantum algorithm needing reversible arithmetic with garbage cleanup
- Understanding resource costs of quantum number-theoretic algorithms

## Complexity

$O(n)$ multiplications of $n$-bit numbers $\bmod N$, each costing $O(n^2)$ gates (schoolbook) or $O(n \log n \log\log n)$ gates (Schönhage-Strassen). Total: $O(n^3)$ or $O(n^2 \log n \log\log n)$.

Space: $O(n)$ qubits.

## Caveat

The garbage cleanup relies on the group structure of $(\mathbb{Z}/N\mathbb{Z})^*$ — specifically that every element has a modular inverse. For non-invertible operations, fall back to general [[Reversible Computation via Compute-Copy-Uncompute|Bennett's method]], which uses more space.

The $O(n^3)$ gate count dominates Shor's algorithm. Optimising this subroutine (windowed arithmetic, Karatsuba multiplication, etc.) directly improves the practical cost of quantum factoring.

## Related notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Reversible Computation via Compute-Copy-Uncompute]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Quantum Fourier Transform Circuit]]
