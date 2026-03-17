
> **Tags:** #trick #qdrift #time-dependent #randomization
> **Source:** arXiv:1906.07115, Section 3

## What it does

Generalizes Campbell's [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]] algorithm to time-dependent Hamiltonians. The core idea: at each step, sample a time $\tau$ from the distribution $p(\tau) \propto \|H(\tau)\|_{\max}$ (proportional to instantaneous norm), sample a term from the decomposition of $H(\tau)$, and apply a short evolution by that term scaled by the integrated norm.

## The construction (1906.07115)

For a $d$-sparse Hamiltonian decomposed as $H(\tau) = \sum_{j=1}^{d^2} H_j(\tau)$ (1-sparse terms from Lemma 3 of the paper):

- Sampling distribution: $p_j(\tau) = \|H(\tau)\|_{\max} / (d^2 \|H\|_{\max,1})$
- Applied evolution: $e^{-i H_j(\tau)/p_j(\tau)}$ = short evolution by the $j$th term normalized to have unit "weight"
- $r$ such steps produce a mixed-unitary channel approximating the true time evolution

The error bound (Theorem 7') is:

$$
\left\|E(t,0) - \prod_j \mathcal{U}(t_{j+1},t_j)\right\|_\diamond \le \frac{4\|H\|_{\max,1}^2}{r},
$$

requiring $r \ge 4\|H\|_{\max,1}^2/\varepsilon$ for error $\varepsilon$.

## Complexity

$O(d^4 \|H\|_{\max,1}^2/\varepsilon)$ queries, $\tilde O(d^4 \|H\|_{\max,1}^2 n/\varepsilon)$ gates (Corollary 9 of 1906.07115).

The quadratic dependence on $\|H\|_{\max,1}$ (integrated norm) is the key weakness relative to the rescaled-Dyson approach.

## When to use

- Time-dependent simulation where simplicity matters more than asymptotic optimality
- Near-term applications where coherent [[Linear Combination of Unitaries (LCU)|LCU]] overhead is prohibitive
- As a baseline comparison for the rescaled-Dyson algorithm

## Caveat

Produces a **mixed-unitary channel**, not a deterministic unitary circuit. This is fine for simulation (error in diamond norm), but differs from coherent [[Linear Combination of Unitaries (LCU)|LCU]] approaches. The quadratic norm scaling is also provably worse than the rescaled-Dyson algorithm for large $\|H\|_{\max,1}/\varepsilon$.

## Related notes
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
- [[Integrated-Norm Equal-Segment Partitioning]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
