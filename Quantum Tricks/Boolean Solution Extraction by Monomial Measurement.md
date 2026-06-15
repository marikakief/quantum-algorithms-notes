# Boolean Solution Extraction by Monomial Measurement

> **Source:** Chen-Gao, arXiv:1712.06239
> **Tags:** #trick #boolean-equation-solving #measurement #HHL

## What it does

Extracts a Boolean satisfying assignment from a monomial pseudo-solution state by repeatedly measuring monomials and fixing the variables that appear in them.

## The trick

Suppose a QLSA routine prepares an approximate pseudo-solution state for
$$
F_B\cup H_X,
\qquad H_X=\{x_1^2-x_1,\ldots,x_n^2-x_n\},
$$
where every algebraic solution is Boolean. The ideal state has amplitudes supported on monomials whose evaluations are nonzero on at least one Boolean solution.

Measure the monomial basis. If the outcome corresponds to
$$
 m=\prod_{i\in S}x_i,
$$
then, except with the QLSA approximation error probability, there is a Boolean solution $a$ with $m(a)=1$. Since $a_i\in\{0,1\}$, this implies
$$
a_i=1\quad\text{for every }i\in S.
$$
So set those variables to $1$, substitute into the system, delete zero equations, and repeat on the remaining variables. If the branch reaches contradiction, restart.

The key estimate is simple. If
$$
\|\,|\widetilde m\rangle-|m\rangle\|^2<\epsilon_1/n,
$$
then the probability of measuring a monomial with zero ideal amplitude is $<\epsilon_1/n$. Over at most $n$ variable-fixing rounds, a run fails with probability $<\epsilon_1$.

## When to reach for it

Use this when a quantum linear-system or algebraic-state routine outputs a monomial-evaluation state and the solution domain is Boolean or idempotent. It avoids asking HHL for a classical vector, which would kill the speedup.

## Complexity

Chen-Gao repeat the inner extraction loop at most $n$ times and restart $\lceil\log_{\epsilon_1}\epsilon\rceil$ times. Choosing $\epsilon_1=1/2$ changes the precision dependence into $O(\log(1/\epsilon))$ restarts rather than demanding a single very accurate HHL call.

Within their Boolean-over-$\mathbb C$ solver, this gives
$$
\widetilde O\!\left(n^{2.5}(n+T_F)\kappa^2\log(1/\epsilon)\right).
$$

## Caveat

The measured monomial may be unhelpful or may lead down a branch that later contradicts the system; the method is Las-Vegas-like with restarts. It also relies on Boolean idempotence: $\prod_{i\in S}a_i\ne0$ must force all selected coordinates to equal $1$.

## Related notes

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]
- [[Macaulay-HHL Pseudo-Solution States]]
- [[Sparse Parity-to-Complex Polynomial Embedding]]
