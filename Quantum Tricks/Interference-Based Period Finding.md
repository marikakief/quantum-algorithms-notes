# Interference-Based Period Finding

> **Source:** Simon, FOCS 1994; Shor, FOCS 1994; generalized in Kitaev (1995)
> **Tags:** #trick #interference #period-finding #query-complexity

## What it does

Uses quantum interference to convert a function with hidden periodicity into a state that's peaked at multiples of the dual period, enabling period extraction with exponentially fewer queries than any classical algorithm.

## The mechanism

A periodic function $f$ with period $s$ produces coset states $\frac{1}{\sqrt{|H|}} \sum_{h \in H} |g_0 + h\rangle$ where $H = \langle s \rangle$. The Fourier transform converts this to:

$$
\text{QFT}: \frac{1}{\sqrt{|H|}} \sum_{h \in H} |g_0 + h\rangle \;\mapsto\; \text{peaked at frequencies } k \text{ with } k \cdot s \equiv 0
$$

**Why it works:** At frequency $k$ with $k \cdot s = 0$, all terms in the sum have the same phase → constructive interference. At frequency $k$ with $k \cdot s \neq 0$, the phases are uniformly distributed on the unit circle → destructive interference (cancellation).

**Over $\mathbb{Z}_2^n$:** The Hadamard transform maps $|x_0\rangle + |x_0 \oplus s\rangle$ to $\sum_{y: y \cdot s = 0} (\pm 1)|y\rangle$. Perfect cancellation because $(-1)^{y \cdot x_0} + (-1)^{y \cdot (x_0 \oplus s)} = 0$ when $y \cdot s = 1$.

**Over $\mathbb{Z}_N$:** The QFT maps a uniform superposition over $\{x_0, x_0 + r, x_0 + 2r, \ldots\}$ to peaks at multiples of $N/r$.

## When to reach for it

- Any problem where the answer is encoded as the period of a function
- Factoring (via order-finding): the period of $a^x \bmod N$
- Discrete logarithm: the period of $(a^x, a^{sx}) \mapsto a^{x + sy}$
- Hidden shift problems
- Understanding why quantum computers can break RSA / Diffie-Hellman

## The classical barrier

Classical algorithms can detect period $r$ in $f: \{0,\ldots,N-1\} \to S$ only by finding collisions ($f(x) = f(x+r)$). This requires $\Omega(\sqrt{N/r})$ queries (birthday bound). For Simon's problem with $r = 2$ and $N = 2^n$, that's $\Omega(2^{n/2})$. The quantum algorithm needs $O(n)$.

The core advantage: quantum interference extracts global structure (periodicity) from a superposition, while classical algorithms must discover it through pairwise comparisons.

## Caveat

Requires coherent evaluation of $f$ — the function must be implementable as a unitary $|x\rangle|0\rangle \mapsto |x\rangle|f(x)\rangle$. If $f$ can only be evaluated classically (e.g., involves a measurement), the interference trick doesn't apply.

Also requires the periodicity to be exact (or very close to exact). For approximate periodicity, more sophisticated techniques are needed (e.g., windowed QFT, robust phase estimation).

## Related notes

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Coset Sampling via Fourier Transform]]
- [[Gapped Phase Estimation]]
