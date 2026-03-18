> **Source:** András Gilyén, Srinivasan Arunachalam, Nathan Wiebe, *Optimizing quantum optimization algorithms via faster quantum gradient computation*, arXiv:1711.00465 (2017/2019)
> **Links:** [arXiv](https://arxiv.org/abs/1711.00465)
> **Tags:** #gradient #optimization #oracle #phase-kickback #VQE #QAOA

---

## What the paper does

Improves [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan's gradient algorithm]] in two ways:

1. **Quadratically better dependence on function evaluation accuracy** for smooth functions — uses higher-order central difference formulas instead of first-order finite differences, reducing the required precision $n_o$ of the oracle from $O(n + d)$ to $O(n + \log d)$ for important function classes.
2. **Efficient conversion between probability and phase oracles** — quantum optimisation problems naturally produce probability oracles ($f(\theta) = |\langle 0 | U(\theta) | 0 \rangle|^2$), but gradient algorithms need phase oracles. The paper gives log-overhead conversions between the two.

Also proves the gradient algorithm is essentially **optimal** in the continuous phase-query model (up to polylog factors) for a class of smooth functions, and shows **exponential speedups over Jordan** for low-degree multivariate polynomials in terms of $d$.

---

## The improvement over Jordan

### Jordan's algorithm recap

[[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan (2005)]]: one query to a phase oracle of $f$ gives the $d$-dimensional gradient. The error scales with the Hessian: $\sigma_i \sim l \sqrt{\sum_j (\partial^2 f / \partial x_i \partial x_j)^2}$ where $l$ is the sampling region size.

The catch: to get $n$ bits of gradient precision, the oracle must evaluate $f$ to $n_o \sim n + O(\log(1/l))$ bits. For smooth functions, $l$ must be small enough that the linear approximation holds, and the required $n_o$ can be large.

### This paper's algorithm

Replace the uniform superposition over a box (which gives first-order differences) with a **higher-order central difference stencil** encoded as a quantum state. Specifically, prepare the input registers in a state whose amplitudes are the coefficients of a $k$-th order central difference formula:

$$|\psi\rangle = \sum_j c_j |\mathbf{x} + j \mathbf{h}\rangle$$

where the $c_j$ are chosen so that the $k$-th order Taylor terms cancel. This is the quantum analogue of using higher-order finite difference stencils classically.

**Result:** For functions with bounded $k$-th derivatives, the gradient can be estimated using $O(\log d)$ queries (not 1, but logarithmic) to the phase oracle, with function evaluation precision $n_o = n + O(\log d)$ — a quadratic improvement over Jordan in the precision requirement.

For low-degree polynomials of degree $p$: only $O(p \log d)$ queries are needed, and the precision requirement is independent of $d$ — exponentially better than Jordan's $O(d)$.

### Lower bound

In the continuous phase-query model, gradient computation requires $\Omega(\log d)$ queries for functions with bounded second derivatives. The algorithm is optimal up to polylog factors.

---

## Oracle conversions

A key practical contribution. Quantum optimisation procedures (VQE, QAOA, quantum autoencoders) naturally produce **probability oracles**: the quantity to optimise is an expectation value $f(\theta) = \langle \psi(\theta) | O | \psi(\theta) \rangle$, accessible by preparing $|\psi(\theta)\rangle$ and measuring. But gradient algorithms need **phase oracles**: $|x\rangle \to e^{i f(x)} |x\rangle$.

The paper gives efficient conversions:
- **Probability → Phase:** Use amplitude estimation to extract $f(\theta)$ as a phase. Cost: $O(\log(1/\varepsilon))$ overhead for $\varepsilon$ precision.
- **Phase → Binary → Fractional Phase:** Chain of reductions between oracle types, each with log overhead.

These conversions use robust oblivious amplitude amplification as a subroutine.

---

## Applications

| Application | Improvement |
|---|---|
| **VQE** | Quadratic reduction in the number of measurements needed per gradient step |
| **QAOA** | Same — fewer circuit evaluations per optimisation iteration |
| **Quantum autoencoders** | Improved training via gradient computation |

For VQE with $d$ parameters: classical gradient estimation needs $O(d/\varepsilon^2)$ measurements. This algorithm reduces the $\varepsilon$-dependence quadratically.

---

## Key results

| Result | Statement |
|---|---|
| Gradient queries (smooth $f$) | $O(\log d)$ phase oracle queries for $n$-bit gradient |
| Precision improvement | $n_o = n + O(\log d)$ vs. Jordan's $n_o = n + O(d)$ for smooth functions |
| Low-degree polynomials | $O(p \log d)$ queries, precision independent of $d$ |
| Lower bound | $\Omega(\log d)$ queries necessary (continuous phase-query model) |
| Oracle conversion | Probability ↔ phase oracle with $O(\log(1/\varepsilon))$ overhead |

---

## Limits / caveats

- The improvement is **quadratic in precision**, not in query count. Jordan's 1-query result still holds for the basic setting; this paper improves the required *quality* of the oracle.
- The $O(\log d)$ query count applies in the improved setting with higher-order stencils — not the single-query regime.
- **Smoothness assumptions are real.** The improvement requires bounded higher derivatives. Many practical functions satisfy this (the paper shows quantum optimisation objectives do), but not all.
- The oracle conversion overhead, while logarithmic, adds to the constant factors. For small problems the overhead may dominate.

---

## Reusable ideas

1. [[Higher-Order Stencil Encoding for Quantum Gradient Estimation]] — encode central difference coefficients as quantum state amplitudes to cancel higher-order Taylor terms, quadratically reducing the oracle precision requirement.
2. [[Probability-to-Phase Oracle Conversion]] — convert a probability oracle (measurement-based, as in VQE/QAOA) to a phase oracle (needed for quantum gradient algorithms) using amplitude estimation with log overhead.

---

## References within this paper

- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan (2005)]] — the algorithm being improved (ref [Jor05])
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — amplitude amplification used in oracle conversions
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo-McClean et al. (2014)]] — VQE, main application
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|Farhi-Goldstone-Gutmann (2014)]] — QAOA, another application
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL (2009)]] — referenced for linear systems context
- Bernstein & Vazirani (1993) — the original discrete algorithm that Jordan generalised

---

## Cross-links

### Paper notes
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]] — the algorithm this directly improves
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — Gilyén's later work; related polynomial transformation techniques
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — main application target
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] — quantum chemistry context

### Trick cards
- [[Higher-Order Stencil Encoding for Quantum Gradient Estimation]]
- [[Probability-to-Phase Oracle Conversion]]
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]] — the phase kickback mechanism both papers use
- [[Phase Kickback from Oracle Queries]]
