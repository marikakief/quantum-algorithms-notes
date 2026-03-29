# Average-Case Amplification via Power-Law Tails

> **Source:** Montanaro, arXiv:1509.02374; also Ambainis-de Wolf, J. Phys. A **34**:6741 (2001)
> **Tags:** #trick #average-case #speedup #Jensen #power-law #complexity

## What it does

Turns a worst-case quadratic quantum speedup into an exponential average-case separation, given the right distribution on problem instances.

## The trick

Suppose a quantum algorithm $Q$ satisfies $T_Q(X) \leq \sqrt{T_C(X)} \cdot \text{poly}(n)$ for all instances $X$, where $T_C(X)$ is the classical runtime. By Jensen's inequality (concavity of $\sqrt{\cdot}$):

$$\mathbb{E}[T_Q(X)] \leq \sqrt{\mathbb{E}[T_Q(X)^2]} \lesssim \sqrt{\mathbb{E}[T_C(X)]}$$

If the classical runtime has a power-law tail $\Pr[T_C = t] \sim t^\beta$ with $-2 < \beta < -3/2$, then:

- $\mathbb{E}[T_C] = \sum_t t \cdot t^\beta = \sum_t t^{\beta+1}$ diverges (since $\beta + 1 > -1$), giving exponentially large expected classical runtime
- $\mathbb{E}[\sqrt{T_C}] = \sum_t \sqrt{t} \cdot t^\beta = \sum_t t^{\beta + 1/2}$ converges (since $\beta + 1/2 < -1$), giving polynomial expected quantum runtime

The sweet spot is $\beta \in (-2, -3/2)$: heavy enough that the classical mean diverges, light enough that the quantum mean converges.

Montanaro demonstrates this for random 3-SAT with a distribution over clause densities $m$: choosing $p_m \propto 2^{-0.454 n^{3/2}/\sqrt{m}}$ yields $\mathbb{E}[T_C] = \Omega(2^{0.094n})$ while $\mathbb{E}[T_Q] = \text{poly}(n)$.

## When to reach for it

- Any setting where you have an instance-dependent quadratic quantum speedup
- The classical runtime distribution has heavy (power-law) tails
- You want to argue for exponential quantum advantage in an average-case model
- This applies beyond backtracking: [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] + power-law instance distributions would also work

## Complexity

Depends on the base algorithm. The amplification itself is free — it's a property of the distribution, not an algorithmic modification.

## Caveat

The distribution must be over *instances*, not over internal randomness of the algorithm. If the power-law tail comes from a randomised classical algorithm running on a single instance (sampling different random seeds), the argument fails: stopping the classical algorithm after $O(R^2)$ time succeeds with constant probability by Markov's inequality.

Also, the specific distributions that give exponential separations may seem contrived. Whether "natural" distributions over real-world CSP instances have the right tail behaviour is an open empirical question.

## Related notes
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
