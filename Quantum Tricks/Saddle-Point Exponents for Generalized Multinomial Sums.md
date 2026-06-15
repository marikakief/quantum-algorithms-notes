# Saddle-Point Exponents for Generalized Multinomial Sums

> **Source:** Boulebnane--Montanaro, arXiv:2208.06909
> **Tags:** #trick #saddle-point #multinomial #QAOA #average-case

## What it does
Extracts the leading exponential scaling of a generalized multinomial sum by converting it to an integral and evaluating the dominant saddle point.

## The trick
After type-count expansion, the QAOA average success probability has the shape

$$
\sum_{\sum_s n_s=n}
\binom{n}{(n_s)_s}\prod_s b_s^{n_s}
\exp\big(n\,P((n_s/n)_s)\big),
$$

where $P$ is a polynomial in the empirical type frequencies. This generalises the standard multinomial theorem.

Boulebnane--Montanaro express such sums as contour integrals. For the random-$2^q$-SAT case and sufficiently small QAOA phase angles, the integral has a unique critical point $z^*$, so

$$
\frac{1}{n}\log S_n \to \Phi(z^*)
$$

for an explicitly computable function $\Phi$.

## Why it matters
It turns average-case QAOA performance into an exponent-computation problem. For random SAT, that means estimating

$$
\mathbb{E}[p_{\mathrm{succ}}]\approx 2^{-c n}
$$

without simulating large instances.

## When to reach for it
Use when the averaged QAOA quantity is symmetric under variable permutations and can be written in terms of empirical type counts.

## Caveat
The proof in this paper needs small enough phase angles to guarantee the saddle point is unique. Numerics suggest useful accuracy beyond the strict theorem, but that part is empirical.

## Related notes
- [[Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) — Paper Notes]]
- [[Poissonized Random-CSP Averaging]]
- [[Type-Count Expansion for QAOA Paths]]
