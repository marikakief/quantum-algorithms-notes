
> **Source:** Shor, arXiv:quant-ph/9508027 (1994), §3; using Bennett (1973) reversible computation
> **Tags:** #trick #reversible-computation #modular-arithmetic #factoring

## What it does

Computes $|a\rangle|1\rangle \mapsto |a\rangle|x^a \bmod N\rangle$ reversibly, cleaning up all garbage bits. This is the bottleneck subroutine in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's factoring algorithm]]. The order-finding branch assumes $\gcd(x,N)=1$; if not, the classical gcd has already found a nontrivial factor.

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

For a known invertible constant $c$, reversible modular multiplication can be made in-place by the swap-and-inverse trick:

$$
|x\rangle|0\rangle
\xrightarrow{\times c}
|x\rangle|cx\bmod N\rangle
\xrightarrow{\mathrm{swap}}
|cx\bmod N\rangle|x\rangle
\xrightarrow{\times c^{-1}}
|cx\bmod N\rangle|0\rangle.
$$

This is the Beauregard/HRS-style cleanup for modular multiplication by known constants. It differs from generic Bennett cleanup, where one copies the output and runs the entire computation backward. The inverse exists because multiplication by $c$ is a permutation of $(\mathbb Z/N\mathbb Z)^*$.

Measuring a register that should have uncomputed to $|0\rangle$ can detect some faults in an idealized reversible basis-state test, but this is not Shor's algorithmic "watchdog" and not a fault-tolerant syndrome-extraction mechanism.

## When to reach for it

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]: the modular exponentiation is the most expensive step
- Any quantum algorithm needing reversible arithmetic with garbage cleanup
- Understanding resource costs of quantum number-theoretic algorithms

## Complexity

For $n=\lceil\log_2 N\rceil$, schoolbook modular exponentiation uses $O(n)$ controlled modular multiplications by classically precomputed constants, with each multiplication built from $O(n)$ controlled additions. With schoolbook adders this gives the familiar $O(n^3)$-gate scale. Faster arithmetic can change asymptotics, but reversible overhead, workspace, and cleanup must be costed explicitly.

Space: $O(n)$ qubits.

## Caveat

The garbage cleanup relies on the group structure of $(\mathbb{Z}/N\mathbb{Z})^*$ — specifically that every element has a modular inverse. For non-invertible operations, fall back to general [[Reversible Computation via Compute-Copy-Uncompute|Bennett's method]], which uses more space.

The $O(n^3)$ gate count dominates Shor's algorithm. Optimising this subroutine (windowed arithmetic, Karatsuba multiplication, etc.) directly improves the practical cost of quantum factoring.

## Related notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Reversible Computation via Compute-Copy-Uncompute]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Quantum Fourier Transform Circuit]]
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — reduces the cost of the underlying multiplier via Karatsuba recursion
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — uses this trick with a Toffoli-based modular multiplier to achieve $2n + 2$ qubit Shor's algorithm
