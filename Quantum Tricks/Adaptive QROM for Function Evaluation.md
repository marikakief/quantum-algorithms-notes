# Adaptive QROM for Function Evaluation

> **Source:** Sanders, Berry, Costa, Tessler, Wiebe, Gidney, Neven, Babbush, arXiv:2007.07391
> **Tags:** #trick #QROM #function-evaluation #arithmetic #heuristic

## What it does
Evaluates an arithmetic function $f(z)$ (e.g., exponential, arcsine) on a quantum register cheaply and approximately, using piecewise linear interpolation with classically precomputed sample points accessed via [[QROM (Quantum Read-Only Memory)|QROM]].

## The trick
Instead of building an exact reversible circuit for $f$ (which requires expensive arithmetic: multipliers, CORDIC, etc.), approximate $f$ by a piecewise linear function $\tilde{f}$:

1. Choose sample points $z_0 < z_1 < \cdots < z_g$ that partition the domain into $g$ intervals.
2. Classically precompute $f(z_\ell)$ for each sample point.
3. At runtime, use QROM to identify which interval $z$ falls in, and output the slope and intercept for that interval.
4. Compute $\tilde{f}(z) = \alpha f(z_\ell) + (1 - \alpha) f(z_{\ell+1})$ via a single multiplication.

The key efficiency gain: **space sample points at exponentially growing intervals** (powers of 2) to match the natural scale of exponential-family functions. This uses a variable-spacing QROM variant where colliding data entries (same slope/intercept) are grouped, and the iteration skips by increasing powers of 2.

The number of distinct regions scales as $g = O(2^{b_{\text{fun}}/2})$, where $b_{\text{fun}}$ is the desired bits of approximation accuracy. The error of piecewise linear interpolation is $\sim (\delta z)^2 |f''(z)| / 8$, so optimal interval width is $\delta z \propto 2^{-b_{\text{fun}}/2} / \sqrt{|f''(z)|}$.

## When to reach for it
- Any heuristic quantum algorithm that needs to evaluate a monotonic function of an oracle output (transition probabilities in QSA, activation functions, Boltzmann weights).
- When the precision requirement is modest (a few bits suffice) and you care more about Toffoli count than exactness.
- When $f$ is smooth and well-approximated by piecewise linear segments.

## Complexity
- Toffoli cost: $b_{\text{sm}}^2 + b_{\text{dif}} + O(b_{\text{sm}} \log b_{\text{sm}} + 2^{b_{\text{fun}}/2})$, dominated by the single multiplication.
- For arcsine (divergent slope at 0): $(b_{\text{sm}} + b_{\text{fun}})^2 + b_{\text{dif}} + O(\cdots)$.
- Ancilla: $2b_{\text{sm}} + O(\log b_{\text{sm}})$ persistent, $b_{\text{dif}} - 1$ temporary.

## Caveat
Not suitable when high precision is needed — the error is polynomial in $2^{-b_{\text{fun}}}$, and the multiplication cost grows quadratically in the output precision. For exact function evaluation or when $b_{\text{fun}} > 20$, dedicated arithmetic circuits (Newton iteration, CORDIC) may be competitive. Also, the variable-spacing QROM requires that region boundaries align with powers-of-2 bit patterns — you can't use arbitrary interval boundaries without paying extra.

## Related notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
