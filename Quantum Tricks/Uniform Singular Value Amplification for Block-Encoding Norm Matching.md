
> **Source:** Fang, Lin & Tong, arXiv:2208.06941, Quantum **7**, 955 (2023). Technique based on [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019), Theorem 17]].
> **Tags:** #trick #QSVT #block-encoding #singular-value-transformation #amplification

## What it does

Rescales a block-encoding's normalisation factor from a loose upper bound $\alpha$ down to $\approx \|\Xi\|$ (the actual operator norm), so that composing many non-unitary block-encodings doesn't incur exponential success-probability decay.

## The trick

Given a $(\alpha, m, 0)$-block encoding $U$ of an operator $\Xi$, where $\alpha \gg \|\Xi\|$, use [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] to apply an odd polynomial $p(x)$ that approximates $x \cdot (\alpha / \|\Xi\|)$ on the interval $[\|\Xi\|/\alpha, 1]$ while staying bounded by 1 on $[-1,1]$. The result is a $(\|\Xi\|/(1-\delta), m+1, \epsilon\|\Xi\|)$-block encoding of $\Xi$.

Specifically, the polynomial approximates:
$$p(x) \approx \frac{x}{\|\Xi\|/\alpha} \quad \text{for } x \in [\|\Xi\|/\alpha - \text{gap}, 1]$$
with $|p(x)| \leq 1$ everywhere on $[-1, 1]$.

**Cost:** $O\!\left(\frac{\alpha}{\delta\|\Xi\|} \cdot \log\frac{\alpha\|\Xi\|}{\epsilon}\right)$ applications of controlled-$U$ and $U^\dagger$.

## When to reach for it

- Composing a sequence of non-unitary block-encodings (e.g., time-marching ODE solvers) where each step has normalisation $\alpha_l \gg \|\Xi_l\|$
- Any setting where the block-encoding normalisation is much larger than the encoded operator's norm, and you need to chain multiple such encodings
- Alternatives to [[Oblivious Amplitude Amplification (Robust)|OAA]] when you want to amplify *all* singular values uniformly rather than a specific one

## Complexity

$O(\alpha / (\delta\|\Xi\|))$ uses of $U$ per amplification step. In the time-marching ODE context, $\alpha = O(1)$ for short segments and $\|\Xi_l\| \leq 1$, so the overhead per segment is $O(1/\delta) = O(L)$ when $\delta = 1/(2L)$.

## Caveat

Requires knowing $\|\Xi\|$ (or a lower bound on it) to design the amplification polynomial. In ODE applications, this is typically available from the short-time propagator bound $\|\Xi_l\| \geq e^{-\|A\|(t_l - t_{l-1})}$. The technique is a special case of QSVT, so it inherits the need for [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|phase-factor computation]].

## Related notes
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Compression Gadget for Sequential Block-Encoding Composition]]
