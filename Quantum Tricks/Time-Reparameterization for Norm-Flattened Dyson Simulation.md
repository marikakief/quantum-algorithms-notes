


> **Tags:** #trick #dyson #time-dependent #lcu
> **Source:** arXiv:1906.07115, Section 4 (Theorems 10–11)

## The idea

Any time-dependent Hamiltonian $H(\tau)$ with nonconstant norm can be reparameterized to give an equivalent one with **unit spectral norm everywhere**. Define the cumulative norm

$$
s = f(t) := \int_0^t d\tau\,\|H(\tau)\|_{\max}
$$

and the rescaled Hamiltonian

$$
\tilde H(s) := \frac{H(f^{-1}(s))}{\|H(f^{-1}(s))\|_{\max}}.
$$

Then $\|\tilde H(s)\|_{\max} = 1$ for all $s$, and $E(t,0) = \mathcal{T}\exp(-i\int_0^S \tilde H(s)\,ds)$ where $S = \|H\|_{\max,1}$.

Simulating $\tilde H$ on $[0, S]$ with an existing Dyson-series algorithm gives L1-norm scaling for $H$.

## Why this achieves near-linear scaling

The Dyson algorithm complexity depends on $\lambda_{\max,1} \cdot K$, where $\lambda_{\max,1}$ is the [[Linear Combination of Unitaries (LCU)|LCU]] normalization parameter (proportional to the integrated max-norm times sparsity) and $K = O(\log(\lambda_{\max,1}/\varepsilon) / \log\log(\cdots))$. After reparameterization, $\lambda_{\max,1} = d\|H\|_{\max,1}$ (the total simulation budget), giving:

$$
\text{Queries} = O\!\left(d\,\|H\|_{\max,1}\cdot\frac{\log(d\,\|H\|_{\max,1}/\varepsilon)}{\log\log(d\,\|H\|_{\max,1}/\varepsilon)}\right).
$$

This is Theorem 10 of 1906.07115 for the sparse model.

## Additional oracle requirements

The rescaled simulation needs two extra oracles beyond the standard $O_{\rm loc}$, $O_{\rm val}$:
- $O_{\rm var}$: computes $f^{-1}(s)$ for given $s$ (inverse cumulative norm)
- $O_{\rm norm}$: computes $\|H(\tau)\|_{\max}$ for given $\tau$

Both can be implemented with $O(\log(t/\delta))$ queries to $O_{\rm val}$ and arithmetic.

## Practical note

If the exact norm $\|H(\tau)\|$ is hard to compute, any efficiently computable upper bound $\Lambda(\tau) \ge \|H(\tau)\|$ works. The total simulation time becomes $S = \int_0^t \Lambda(\tau)\,d\tau$ instead, which may be somewhat larger.

## Related notes
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
- [[Integrated-Norm Equal-Segment Partitioning]]
- [[Continuous qDRIFT Time-Sampling by Instantaneous Norm]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
