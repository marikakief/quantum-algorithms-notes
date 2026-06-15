# Quantum Basin Hopping with Gradient-Based Local Optimisation (Bulger 2005)

> **Source:** David Bulger, *Quantum basin hopping with gradient-based local optimisation*, arXiv:quant-ph/0507193 (2005)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0507193) · [PDF](https://arxiv.org/pdf/quant-ph/0507193)
> **Tags:** #quantum-algorithm #optimization #Grover #gradient-estimation #global-optimisation

---

## The computational problem

Given a continuous, unconstrained objective function $f : D \to \mathbb{R}$ on a finite discretisation $S \subset D \subset \mathbb{R}^p$, find a global minimum or a basin whose local minimum lies below a threshold $Y$.

The model is black-box-ish but not purely unstructured: the algorithm may evaluate $f$ coherently and may run a local descent routine for $L$ steps. The cost comparison is against:

- pure random search over $S$;
- classical multistart local optimisation;
- Grover-style minimum search / pure adaptive search.

The complexity measure is mainly oracle effort: Grover gives the usual square-root dependence on the number of basins, while [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan gradient estimation]] replaces the classical $O(p)$ function evaluations per gradient by one coherent function query, up to precision overheads.

## What the paper does

Bulger combines the earlier quantum basin hopper with [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan's one-query gradient estimation]]. The claim is not a new asymptotic global-optimisation miracle: it is that the basin-hopping Grover wrapper survives the non-determinism of quantum gradient estimates.

The useful contribution is the error model. Gradient-estimation error is split into low-amplitude *large* errors and bounded *small* errors. Large errors can be made negligible by precision choice; small errors create a third, entangling boundary region in Grover search, but the success probability remains high when that boundary is much smaller than the marked basin set.

## The algorithm / construction

### Quantum basin hopping

The outer algorithm is classical control around a Grover subroutine. Start from a random point, descend to a local minimum, and maintain the current best threshold $Y$. Then repeatedly call a Grover basin-search routine with a random number of Grover rotations, increasing the search scale as in unknown-target-size search.

The quantum subroutine $\mathrm{GBS}(Y,r,L)$:

1. Prepare the first domain register in the uniform state $|s\rangle$ over $S$ and initialise $L$ further domain registers plus an output register.
2. Apply $L$ local-descent updates
   $$
   U_{d,L}\cdots U_{d,1},
   $$
   so that an initial point $x_0$ maps to a local-search path $x_1,\ldots,x_L$.
3. Evaluate $f(x_L)$ into the range register with $U_f$.
4. Apply a phase flip $U_{<}$ when $f(x_L) < Y$.
5. Uncompute the local-search path and apply the diffusion reflection $U_s$ on the initial-point register.
6. Repeat the Grover iterate $r$ times, then measure the terminal point and objective value.

In the deterministic-local-search version, the marked set is exactly the set of initial points whose basin descends below $Y$.

### Adding quantum gradient estimation

Here each local update $U_{d,k}$ uses one quantum gradient estimate. The operator in one call to $\mathrm{GBS}(Y,r,L)$ contains $(2r+1)L$ such estimates because the Grover iterate computes and uncomputes the local search.

Quantum gradient estimation does not return a single classical gradient inside the coherent computation. It generally produces a superposition of estimates. That matters because the threshold oracle $U_{<}$ can then entangle the initial-point register with the gradient-estimation work registers.

Bulger handles this by choosing an error tolerance $\delta$:

- **Large errors:** estimates with gradient error $>\delta$.
- **Small errors:** estimates with gradient error $\le \delta$.

For large errors, Theorem 1 of Bulger's preceding gradient-estimation analysis says the amplitude outside the good-estimate subspace can be made $<\epsilon$. If $|\phi\rangle$ is the actual final state and $|\phi'\rangle$ is the state after projecting every estimate into the good subspace, then

$$
\lVert |\phi'\rangle - |\phi\rangle \rVert < (2r+1)L\epsilon.
$$

So large errors are harmless provided the gradient-estimation precision is set so that $\epsilon \ll 1/((2r+1)L)$.

### The three-region Grover analysis

For small errors, Bulger partitions the initial points into three sets. Let $d_L(x_0,u_1,\ldots,u_L)$ be the endpoint after $L$ local-search steps when the gradient errors are $u_k$.

Define the deterministic regions:

$$
W_+ = \{x : d_L(x,0,\ldots,0) > Y\}, \qquad
W_Y = \{x : d_L(x,0,\ldots,0) = Y\}, \qquad
W_- = \{x : d_L(x,0,\ldots,0) < Y\}.
$$

Then define the uncertain boundary region

$$
W_{Y,\delta} = \{x : d_L(x,B_\delta,\ldots,B_\delta) \text{ contains values both below and above } Y\}.
$$

On the discretisation $S$:

$$
S_\beta = S \cap W_{Y,\delta}, \qquad
S_\alpha = (S\setminus S_\beta) \cap W_+, \qquad
S_\gamma = (S\setminus S_\beta) \cap W_-.
$$

Here $S_\gamma$ is marked, $S_\alpha$ is unmarked, and $S_\beta$ is the dangerous boundary where small gradient errors can flip the threshold comparison and entangle the work space.

The Grover operator is no longer a clean two-dimensional rotation. Still, when $N_\beta \ll N_\gamma \ll N_\alpha$, using the usual Grover scale

$$
k \approx \frac{\pi}{4}\sqrt{\frac{N}{N_\gamma}}
$$

gives success amplitude close to the ideal one, with an error rate of order

$$
O\!\left(\sqrt{\frac{N_\beta}{N_\gamma}}\right).
$$

That is the paper's central technical point: coherent gradient-estimation noise only spoils the basin search in proportion to the square root of the uncertain boundary fraction relative to the genuinely good basins.

## Key results

- **Quantum basin search over basins:** the basin hopper finds a basin extending below the threshold with Grover-type effort proportional to the square root of the relevant basin count, rather than the number of initial points.
- **Gradient-evaluation speedup:** using quantum gradient estimation reduces the per-gradient query dependence from linear in dimension $p$ to constant query count, assuming the function-evaluation oracle and precision requirements are acceptable.
- **Large-error stability:** after $(2r+1)L$ gradient estimates, the state perturbation from large-error components is bounded by $(2r+1)L\epsilon$.
- **Small-error stability:** if the uncertain boundary set $S_\beta$ is much smaller than the marked basin set $S_\gamma$, the Grover subroutine still succeeds with high probability; the induced error is of order $\sqrt{N_\beta/N_\gamma}$.

## Comparison with prior work

| Work | Role here | What changes |
|---|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] | amplitude amplification over initial points | used as the basin-search engine |
| [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|BBHT (1998)]] | unknown target count | randomised iteration counts handle unknown number of improving basins |
| [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr-Høyer (1996)]] | threshold-based optimisation | basin hopping applies threshold search to local-minimum basins rather than table entries |
| Bulger (2004) | quantum basin hopper with deterministic local search | this paper replaces deterministic local search by gradient-based quantum local search |
| [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan (2005)]] | one-query numerical gradient | supplies the dimension-factor speedup inside each local step |

## Limits / caveats

- This is an early query-complexity argument, not an implementable optimisation package. The oracle assumptions and coherent local-search machinery are strong.
- The speedup depends on local search being useful. If the objective has many tiny basins or a large threshold boundary, the basin picture stops helping.
- The bound needs $N_\beta \ll N_\gamma$. If the threshold cuts through a large fuzzy boundary, gradient-estimation entanglement can spoil the Grover rotation.
- The paper does not optimise constants or give a practical recipe for choosing precision, $\delta$, $L$, or the sampling scale. Bulger explicitly points to this as future work.
- For non-smooth or badly conditioned landscapes, [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan-style gradient estimation]] is not obviously cheap enough to matter.

My assessment: cute as a piece of early algorithmic plumbing, but not a reason to believe black-box nonconvex optimisation becomes easy. The good reusable idea is the three-region treatment of coherent oracle errors.

## Reusable ideas

1. [[Quantum Basin Hopping via Basin Membership Oracles]] — convert local descent into a Grover-search predicate over basins rather than raw points.
2. [[Three-Way Grover Analysis for Entangling Oracle Errors]] — replace the marked/unmarked Grover decomposition by marked/unmarked/boundary regions when coherent subroutine error can entangle the work space.

## References within this paper

- Baritompa, Bulger & Wood (2005) — Grover applied to global optimisation / pure adaptive search.
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp (1998)]] — unknown target-size Grover search.
- Brooks (1958) — classical random search baseline.
- Bulger, Baritompa & Wood (2003) — implementing pure adaptive search with Grover.
- Bulger (2004) — prior quantum basin hopper with deterministic local search.
- Bulger (2005), *Quantum computational gradient estimation* — error analysis for gradient estimation used here.
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr-Høyer (1996)]] — minimum finding via thresholded Grover search.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — search subroutine.
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan (2005)]] — one-query gradient estimation.
- Nayak & Wu (1999) — quantum query complexity of median and related statistics.
- Press et al. (1995), *Numerical Recipes in C* — classical local optimisation methods.
- Zabinsky & Smith (1992), Zabinsky et al. (1995) — pure adaptive search in finite/global optimisation.

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]]
- [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes]]

### Trick cards

- [[Quantum Basin Hopping via Basin Membership Oracles]]
- [[Three-Way Grover Analysis for Entangling Oracle Errors]]
- [[Randomised Iteration Count for Grover Search]]
- [[Grover Search with Evolving Threshold for Minimum Finding]]
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]]
