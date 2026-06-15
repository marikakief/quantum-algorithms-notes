# Random-Subset Collision Counting for Frequency Moments

> **Source:** Montanaro, arXiv:1505.00113; Bar-Yossef sample-complexity estimator
> **Tags:** #trick #frequency-moments #k-distinctness #collision-counting #query-complexity

## What it does

Estimates $F_k$ by sampling a random subsequence and counting its $k$-wise collisions.

## The trick

For random indices $s_1,\ldots,s_\ell\in[n]$, define

$$
C_k=\left|\left\{T\in\binom{[\ell]}{k}:a_{s_i}=a_{s_j}\text{ for all }i,j\in T\right\}\right|.
$$

Then

$$
\mathbb E[C_k]=\binom{\ell}{k}\frac{F_k}{n^k}.
$$

Choose

$$
\ell=\Theta(n/F_k^{1/k})
$$

so a random subsequence has a constant number of $k$-collisions. Quantumly, find these collisions using a [[Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011) — Paper Notes|$k$-distinctness]] subroutine rather than scanning all tuples.

## When to reach for it

- Estimating high moments or heavy-tail statistics through collisions.
- Problems where the moment is exactly an equality-pattern count.
- Quantum query algorithms that can replace exhaustive collision search by element- or $k$-distinctness.

## Complexity

With Belovs' $k$-distinctness exponent

$$
\alpha_k=1-\frac{2^{k-2}}{2^k-1},
$$

Montanaro gets

$$
\widetilde O\!\left(\frac{n^{(1-1/k)\alpha_k}}{\epsilon^2}\right)
$$

queries for fixed $k>1$.

## Caveat

The algorithm only has expected runtime control in the collision-enumeration phase, which blocks a direct replacement of the $1/\epsilon^2$ averaging by quantum mean estimation.

## Related notes

- [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]]
- [[Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
