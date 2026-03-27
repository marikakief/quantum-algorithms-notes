
> **Source:** Ding, Li, Lin, Ni, Ying, Zhang, arXiv:2402.01013
> **Tags:** #trick #spectral-estimation #Gaussian-filter #Hadamard-test #phase-estimation

## What it does

Converts Hadamard-test time-series data into a filtered spectral density with Gaussian-shaped peaks at each eigenvalue, enabling peak-finding for eigenvalue estimation.

## The trick

Instead of sampling evolution times $t$ uniformly or on a fixed grid, draw them from a truncated Gaussian distribution $a(t)$ with standard deviation $T$ and truncation at $\sigma T$:

$$a(t) \propto e^{-t^2/(2T^2)} \cdot \mathbf{1}_{|t| \leq \sigma T}$$

Then compute the empirical filtered density:

$$G(\theta) = \left|\frac{1}{N} \sum_{n=1}^N Z_n \, e^{i\theta t_n}\right|$$

where $Z_n$ are [[Approximate CDF from Hadamard Tests|Hadamard-test]] measurement outcomes at times $t_n \sim a(t)$.

The expected value is $\hat{G}(\theta) = \sum_m p_m F(\theta - \lambda_m)$ where $F$ is the Fourier transform of $a(t)$. For the Gaussian choice, $F(x) \approx e^{-T^2 x^2/2}$ — each eigenvalue $\lambda_m$ produces a Gaussian bump of width $\sim 1/T$ and height $\sim p_m$.

The width-height trade-off is controlled by $T$: larger $T$ means narrower peaks (better resolution) but longer maximal circuit depth $T_{\max} = \sigma T$.

## When to reach for it

- Multiple eigenvalue estimation from a single dataset — each eigenvalue creates a distinct peak.
- When you want a smooth, well-behaved filter function without sidelobe artifacts (Gaussian has no sidelobes, unlike rectangular or sinc-based filters).
- Early fault-tolerant settings where you need to balance resolution against circuit depth.
- As a simpler alternative to ESPRIT or matrix-pencil methods when approximate peak locations suffice.

## Complexity

$N = \tilde{O}((p_{\min} - p_{\text{tail}})^{-2} \cdot \log(|D|/\eta))$ samples for reliable peak detection. Each sample requires one [[Approximate CDF from Hadamard Tests|Hadamard test]] with evolution time drawn from $a(t)$. Classical post-processing (computing $G(\theta)$ on a grid) costs $O(NJ)$ where $J = O(T/q)$ is the grid size.

## Caveat

The Gaussian filter has infinite support in theory — truncation at $\sigma T$ introduces a small error that must be accounted for (the truncation parameter $\sigma$ appears in the required lower bounds). The filter also cannot distinguish eigenvalues closer than $\sim 1/T$, so very close eigenvalues require large $T$ and correspondingly deep circuits. When eigenvalues are unresolvable, the algorithm still guarantees a covering (each detected peak is within $\alpha/T$ of some true eigenvalue) but not a one-to-one correspondence.

## Related notes
- [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes]]
- [[Approximate CDF from Hadamard Tests]]
- [[Greedy Peak Search with Blocking]]
- [[Importance Sampling for Fourier CDF Estimation]]
