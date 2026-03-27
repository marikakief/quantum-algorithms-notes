
> **Source:** Jordan, Shutty, Wootters, Zalcman, Schmidhuber, King, Isakov, Khattar, Babbush, arXiv:2408.08292
> **Tags:** #trick #quantum-optimization #coding-theory #reduction #code-duality #DQI

## What it does

Maps any max-LINSAT optimization instance to a syndrome decoding problem on the dual linear code, creating a systematic bridge between the constraint satisfaction and error-correction literatures.

## The trick

Given a max-LINSAT instance defined by matrix $B \in \mathbb{F}_p^{m \times n}$, the dual code is $C^\perp = \{\mathbf{d} \in \mathbb{F}_p^m : B^T \mathbf{d} = \mathbf{0}\}$. The parity check matrix of $C^\perp$ is $B^T$.

The mapping:

| Optimization feature | Coding theory dual |
|---|---|
| Constraint matrix $B$ | Parity check matrix $B^T$ |
| Sparse $B$ (bounded degree) | LDPC code |
| Vandermonde $B$ | Reed-Solomon code |
| Multivariate evaluation $B$ | Reed-Muller code |
| Folded evaluation | Folded Reed-Solomon code |
| Approximation quality target | Decoding radius |

This isn't just an analogy — the DQI algorithm literally solves a decoding instance as its bottleneck step. Existing decoder implementations (Berlekamp-Massey for Reed-Solomon, belief propagation for LDPC, majority logic for Reed-Muller) become subroutines of a quantum optimization algorithm.

The reduction also runs the other direction: information-theoretic limits on decoding (Shannon limit, sphere-packing bounds) yield upper bounds on what DQI can achieve, which can be compared against classical optimization heuristics to delineate regimes of potential quantum advantage.

## When to reach for it

- You're working on a combinatorial optimization problem expressible as linear constraints over a finite field.
- You want to identify whether existing decoding algorithms yield a quantum speedup for your problem via the [[DQI Semicircle Law for Optimization-Decoding Duality|semicircle law]].
- You want to prove limitations on quantum optimization by importing Shannon-type impossibility results.
- You're designing new codes and want to know what optimization problems they'd help solve quantumly.

## Complexity

The reduction itself is free (it's a reinterpretation of the same matrix). The cost of the quantum algorithm is dominated by the reversible decoder circuit. For Reed-Solomon + Berlekamp-Massey at block length $p$: $\sim 10^8$ Toffolis at $p = 521$. For LDPC + belief propagation: depends on graph structure and convergence, but typically polynomial in $m$.

## Caveat

The reduction is useful only when the dual code has efficient decoders reaching a decoding radius that, via the semicircle law, exceeds what classical optimizers can achieve. For many natural problem structures (e.g., dense random $B$), the dual code has no known efficient decoder, and the reduction provides no advantage. Also, the decoder must be implementable as a reversible quantum circuit, adding overhead.

## Related notes
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]
- [[DQI Semicircle Law for Optimization-Decoding Duality]]
- [[QFT-Based Amplitude Shaping via Polynomial Evaluation]]
- [[Uncomputation via Syndrome Decoding]]
