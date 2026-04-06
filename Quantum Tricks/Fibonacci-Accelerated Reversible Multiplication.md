# Fibonacci-Accelerated Reversible Multiplication

> **Source:** Burge, Barbeau, Garcia-Alfaro, arXiv:2411.14434 (2024)
> **Tags:** #trick #reversible-arithmetic #quantum-arithmetic #CORDIC

## What it does
Reversibly multiplies a quantum register by $(1 + 2^{-m})$ using only additions and right-shifts — no multiplications, no ancilla-hungry lookup tables.

## The trick

Given input register `in` (value $z$) and a near-zero auxiliary register `aux`:

1. Set `aux` $\leftarrow$ `aux` + `in`
2. For $i = J, J-1, \ldots, 0$ (where $J = \lfloor\sqrt{5}\,\phi^{-m} n\rfloor$):
   - If $i$ even: `in` $\leftarrow$ `in` $- (-1)^{F[i]}($`aux` $\gg m \cdot F[i])$
   - If $i$ odd: `aux` $\leftarrow$ `aux` $- (-1)^{F[i]}($`in` $\gg m \cdot F[i])$
3. Set `aux` $\leftarrow$ `aux` $-$ `in`

Here $F[i]$ is the $i$th Fibonacci number and $\phi = (1+\sqrt{5})/2$ is the golden ratio.

**Why it works:** The inverse (`Div`) computes partial sums of the geometric series $\sum_{k=0}^{K}(-2^{-m})^k = (1+2^{-m})^{-1}$, but accumulates them via Fibonacci-indexed alternation between two registers. This makes every step reversible (each register is updated using only the other's current value). The Fibonacci indexing gives exponentially fast convergence — after $J$ steps, the error is $O(2^{-m\phi^{J-1}})$ bits.

**Key property:** Each step is just an addition of a right-shifted register. Right-shifting by $k$ bits in fixed-point two's complement is free (it's just a re-wiring of qubit lines). So the entire multiplication is a sequence of plain quantum additions.

## When to reach for it
- Implementing [[CORDIC as Reversible Angle Computation|CORDIC]] on a quantum computer (this is where it was developed — each CORDIC iteration needs multiplication by $(1 + 4^{-i})$ for stretch compensation)
- Any reversible circuit needing multiplication by a value close to 1, where the deviation is a negative power of 2
- Quantum arithmetic on fixed-point registers where you want to avoid full multiplier circuits

## Complexity
For $n$-bit precision with shift parameter $m$: at most $\lfloor\sqrt{5}\,\phi^{-m}n\rfloor + 2$ additions. Each quantum addition costs $O(n)$ CNOTs (ripple-carry) or $O(\log n)$ depth with $O(n)$ ancillas (parallel). For large $m$ (as in later CORDIC iterations), the number of required additions drops rapidly.

## Caveat
- Only works for factors of the form $(1 + 2^{-m})$. General multiplication still requires full multiplier circuits.
- Needs a clean auxiliary register of size $n$, which accumulates garbage during the computation. The register returns to near-zero ($O(2^{-m\phi^{J-1}})$ error) after completion — close enough for fixed-point arithmetic but not exactly zero. In [[CORDIC as Reversible Angle Computation|CORDIC]], this is handled by the overall uncomputation pass.
- The Fibonacci convergence rate means the number of additions is $O(\log_\phi(n/m))$, which is very fast, but the constant factors in the additions themselves ($O(n)$ CNOTs each) dominate at moderate $n$.

## Related notes
- [[Quantum CORDIC — Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024) — Paper Notes]] — source paper
- [[CORDIC as Reversible Angle Computation]] — the algorithm that uses this multiplication primitive
- [[Inequality-Test Amplitude Transduction]] — alternative approach that avoids the need for arcsine (and thus CORDIC) entirely
