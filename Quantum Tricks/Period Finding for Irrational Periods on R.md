# Period Finding for Irrational Periods on R

> **Source:** Sean Hallgren, STOC 2002 / JACM 54(1), 2007
> **Tags:** #trick #period-finding #number-theory #QFT

## What it does
Extends quantum period finding from finite cyclic groups (as in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]) to functions on $\mathbb{R}$ with irrational periods.

## The trick
Given a function $f: \mathbb{R} \to X$ with period $R$ (possibly irrational), one-to-one within each period, and efficiently computable:

1. **Discretise:** Evaluate $f$ at integer multiples of $1/N$ for large $N$, producing $\tilde{f}_N: \mathbb{Z} \to X$
2. **Weak periodicity:** The discretised function won't be exactly periodic (because $NR$ is irrational), but it's *weakly periodic*: for most $k$ and all $l$, either $\tilde{f}(k + \lfloor lNR \rfloor)$ or $\tilde{f}(k + \lceil lNR \rceil)$ equals $\tilde{f}(k)$. The fraction of "bad" $k$ values is at most $1/\text{poly}(n)$ where $n$ is the input size.
3. **Quantum Fourier sampling:** Prepare the superposition $\frac{1}{\sqrt{q}}\sum_m |m\rangle |\tilde{f}(m)\rangle$ with $q \geq 3(NR)^2$, measure the function value, apply [[Quantum Fourier Transform Circuit|QFT]] mod $q$, and measure. The output $j$ concentrates near multiples of $q/(NR)$.
4. **Classical post-processing:** Two independent samples yield values $c, d$. The [[Order-Finding via QFT and Continued Fractions|continued fraction expansion]] of $c/d$ reveals $NR$ to within $\pm 1$.

The key insight is that weak periodicity suffices — the QFT output probabilities are robust to the $O(1/\text{poly})$ fraction of positions where weak periodicity fails.

## When to reach for it
- When the computational problem reduces to finding a period, but the period is irrational or the underlying group is continuous (e.g., $\mathbb{R}$) rather than finite cyclic
- Problems in algebraic number theory where the "period" is a regulator, class number, or other real-valued invariant
- Hidden subgroup problems on groups that are extensions of $\mathbb{Z}$ or $\mathbb{R}$

## Complexity
$\text{poly}(\log S, \log N)$ where $S = NR$ is the discretised period and $N$ controls accuracy. Success probability $1/\text{poly}(\log S)$ per run; standard repetition and amplification apply.

## Caveat
- Requires the function to be efficiently computable at the discretisation points *and* one-to-one within each period
- Needs a way to test whether an integer $m$ is close to an integer multiple of the period (the "verification oracle"), which uses problem-specific structure
- The discretisation parameter $N$ must be fine enough relative to the minimum distance between distinct function values — for Hallgren's application, this requires $1/N < d_{\min}/\log d$ where $d_{\min} = 3/(32D)$

## Related notes
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]]
- [[Infrastructure of Real Quadratic Fields as Period Domain]]
- [[Interference-Based Period Finding]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Quantum Fourier Transform Circuit]]
