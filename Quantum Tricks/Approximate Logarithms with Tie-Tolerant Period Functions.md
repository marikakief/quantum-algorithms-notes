# Approximate Logarithms with Tie-Tolerant Period Functions

> **Source:** Arthur Schmidt, arXiv:0912.4807
> **Tags:** #trick #period-finding #number-theory #precision-management

## What it does

Avoids exact real-logarithm comparisons in infrastructure period functions by allowing a controlled ambiguity zone at transition points.

## The trick

Real-quadratic regulator algorithms need to decide which reduced form lies immediately to the left of a real position $x$. Exact distances use logarithms such as
$$
\operatorname{Log}(\alpha)=\frac12\ln|\sigma(\alpha)/\alpha| \pmod R,
$$
and there is no a priori fixed precision that resolves every possible tie near a boundary. Increasing precision until a tie breaks can destroy a fixed polynomial runtime bound.

Schmidt's fix is not to resolve every tie. Define the period function using approximate logarithms, and prove that the induced error is bounded:
$$
|\delta(\widetilde{g}_{x/4})-\widetilde{\delta}(\widetilde{g}_{x/4})|<1/8.
$$
The function may choose either neighbouring form near a transition, but the positive-$a$ spacing gives enough room:
$$
\delta(\rho^2(g))-\delta(g)>\ln 2>1/4.
$$
So ambiguity is confined to short boundary intervals. The resulting function remains periodic and has bounded many-to-one runs, which is enough for QFT sampling.

## When to reach for it

- Period-finding reductions over real-valued domains where exact comparison is impossible or expensive.
- Algorithms whose natural oracle is a nearest-left/nearest-right map with boundary instability.
- Any construction where a small number of bad or ambiguous points per period can be absorbed into the Fourier analysis.

## Complexity

For $x<\Delta^2$, Schmidt's square-and-multiply form navigation takes at most $c\log_2\Delta$ operations with $c<10$. Choosing each logarithm to precision at least
$$
\frac{1}{8c\log\Delta}
$$
keeps total distance error below $1/8$, so the oracle remains polynomial-time computable in $\log\Delta$.

## Caveat

The trick depends on having a margin larger than the approximation error. If adjacent objects can be closer than the error tolerance, the ambiguity zones can overlap and the period structure may no longer be usable.

## Related notes

- [[Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009) — Paper Notes]]
- [[Many-to-One Period Fibres with Bounded Run-Length Variation]]
- [[Positive-Coefficient Quadratic Forms for Constant-Spaced Infrastructure Sampling]]
- [[Period Finding for Irrational Periods on R]]
