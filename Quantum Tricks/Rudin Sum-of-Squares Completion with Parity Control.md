
> **Source:** Berry, Motlagh, Pantaleoni, Wiebe, arXiv:2401.10321; underlying technique from Rudin, *Amer. Math. Monthly* **107**, 813 (2000)
> **Tags:** #trick #QSP #GQSP #polynomials #completion #sum-of-squares

## What it does

Decomposes a non-negative even polynomial into a sum of two squared polynomials with prescribed parities — one even, one odd — enabling the [[Polynomial Completion via Sum-of-Squares|polynomial completion step]] of [[Generalized Quantum Signal Processing (GQSP)|GQSP]] while maintaining Fourier-parity constraints.

## The trick

Given the completion problem: find real polynomials $P', Q'$ such that

$$P'^2 + Q'^2 = 1 - \alpha^2(P_K^2 - Q_K^2)$$

where $P_K, Q_K$ are the truncated [[Jacobi-Anger Truncation for QSP Simulation|Jacobi–Anger]] polynomials and $P'$ must be even, $Q'$ must be odd (in $x = \cos\theta$).

Since the right-hand side $p(x) = 1 - \alpha^2(P_K^2 - Q_K^2)$ is:
1. Non-negative for all $x$ (proved in Theorem 3 of the paper, including $|x| > 1$)
2. An even polynomial in $x$

Use Rudin's factorization: any non-negative polynomial can be written as

$$p(x) = \left(\text{Re}(q(x))\right)^2 + \left(\text{Im}(q(x))\right)^2$$

where $q(x) = \prod_j (x - c_j) \prod_k (x - a_k + ib_k)$ runs over the roots.

Since $p(x)$ is even, every root $r$ has a corresponding root $-r$. This means $q(x)$ contains paired factors:

- Real roots: $(x - c_j)(x + c_j) = x^2 - c_j^2$ — real part even, imaginary part zero
- Complex roots: $(x - a_k + ib_k)(x + a_k + ib_k) = (x^2 - a_k^2 - b_k^2) + 2ib_kx$ — real part even, imaginary part odd

When multiplying such factors, the parity structure is preserved:

$$(\text{even} + i \cdot \text{odd}) \times (\text{even} + i \cdot \text{odd}) \to (\text{even} + i \cdot \text{odd})$$

So at the end, $\text{Re}(q)$ is even and $\text{Im}(q)$ is odd — exactly the parities needed for $P'$ and $Q'$.

## When to reach for it

- Any [[Generalized Quantum Signal Processing (GQSP)|GQSP]] construction where the polynomial pair needs to satisfy exact unitarity ($|P|^2 + |Q|^2 = 1$) and specific parity constraints simultaneously.
- Extending [[Polynomial Completion via Sum-of-Squares|standard QSP completion]] to handle Laurent-polynomial / complex-polynomial settings.
- General polynomial synthesis problems where the sum-of-squares decomposition must respect symmetry.

## Complexity

Classical preprocessing: find roots of a degree-$\sim 4K$ polynomial, then multiply the paired factors. Root-finding to sufficient precision is $O(\text{poly}(K) \log(K/\epsilon_{\text{classical}}))$. This is a one-time classical cost, negligible compared to the quantum circuit execution.

## Caveat

Requires proving that $1 - \alpha^2(P_K^2 - Q_K^2) \geq 0$ for **all** $x$, not just $|x| \leq 1$. The paper proves this via bounds on Bessel function tails (Theorem 3, Appendix A) — it's not a generic property, it depends on the specific structure of the [[Jacobi-Anger Truncation for QSP Simulation|Jacobi–Anger polynomials]]. For other target functions, the non-negativity would need a separate proof.

## Related notes

- [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]] — the paper that uses this construction
- [[Polynomial Completion via Sum-of-Squares]] — the standard (parity-unaware) version
- [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]] — related root-grouping technique for QSP complementary polynomials
- [[Jacobi-Anger Truncation for QSP Simulation]] — the polynomials being completed
- [[Directional Walk Control for Phase Doubling]] — the trick that necessitates parity-controlled completion
