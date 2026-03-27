
> **Source:** Jordan, Shutty, Wootters, Zalcman, Schmidhuber, King, Isakov, Khattar, Babbush, arXiv:2408.08292
> **Tags:** #trick #quantum-optimization #coding-theory #decoding #semicircle-law #DQI

## What it does

Converts any error-correction guarantee for a linear code into a quantitative approximation guarantee for the dual optimization problem, via a closed-form "semicircle" relation.

## The trick

Given a max-LINSAT instance with $m$ constraints over $\mathbb{F}_p$, each constraint set of size $r$, and a decoder that corrects $\ell$ errors on the dual code $C^\perp$, the expected fraction of satisfied constraints under DQI measurement is:

$$\frac{\langle s \rangle}{m} = \left(\sqrt{\frac{\ell}{m}\left(1 - \frac{r}{p}\right)} + \sqrt{\frac{r}{p}\left(1 - \frac{\ell}{m}\right)}\right)^2$$

In the balanced case $r/p = 1/2$, this becomes:

$$\frac{\langle s \rangle}{m} = \frac{1}{2} + \sqrt{\frac{\ell}{m}\left(1 - \frac{\ell}{m}\right)}$$

The optimal polynomial $P$ achieving this bound can be precomputed classically in polynomial time. The mapping is:

- **Decoding radius** $\ell/m$ → **approximation quality** $\langle s \rangle / m$
- **Code family** (LDPC, Reed-Solomon, Reed-Muller, ...) → **optimization problem class** (sparse XORSAT, OPI, multivariate OPI, ...)

## When to reach for it

- You have a combinatorial optimization problem that can be expressed as max-LINSAT.
- You want to know whether DQI can outperform classical algorithms on it.
- You've found a new decoding result in the coding theory literature and want to translate it into a quantum optimization guarantee.
- You need to compute information-theoretic upper bounds on what DQI can achieve (plug in the Shannon limit for the decoding radius).

## Complexity

The formula itself is $O(1)$ to evaluate. The DQI algorithm that achieves it costs $O(T + m^2)$ gates where $T$ is the decoder cost. The $m^2$ term comes from [[Dicke State Preparation via Inequality Testing|Dicke state preparation]].

## Caveat

The formula assumes either (a) $2\ell + 1 < d^\perp$ (perfect decoding within half the distance), or (b) the decoder succeeds with high probability on random errors (Theorem 10.1 extension). For structured or adversarial instances, the actual performance may differ. Also, the polynomial $P$ must be realized via a reversible decoder circuit — the formula holds only if decoding is efficient.

## Related notes
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]
- [[QFT-Based Amplitude Shaping via Polynomial Evaluation]]
- [[Optimization-to-Decoding Reduction via Code Duality]]
- [[Uncomputation via Syndrome Decoding]]
