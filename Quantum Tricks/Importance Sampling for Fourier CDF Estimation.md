
> **Source:** Lin Lin and Yu Tong, arXiv:2102.11340 (PRX Quantum 3, 010318, 2022)
> **Tags:** #trick #importance-sampling #Fourier #spectral-measure #variance-reduction

## What it does

Evaluates a trigonometric polynomial of eigenvalues from random Hadamard-test measurements, with total cost scaling logarithmically in the polynomial degree rather than linearly.

## The trick

You want to estimate $\tilde{C}(x) = \sum_{|j| \leq d} \hat{F}_j e^{ijx} \operatorname{Tr}[\rho\, e^{-ij\tau H}]$, where $\hat{F}_j$ are Fourier coefficients of a smoothed step function and each $\operatorname{Tr}[\rho\, e^{-ij\tau H}]$ is accessible from a Hadamard test at evolution time $|j|\tau$.

Naive approach: run one Hadamard test for each $j \in \{-d,\ldots,d\}$ and sum. Cost: $O(d)$ circuit executions, each at a different evolution time.

Better: **importance sample** over $j$. Draw $J$ from $\{-d,\ldots,d\}$ with probability $|\hat{F}_j| / F_1$ where $F_1 = \sum |\hat{F}_j|$, then form:

$$G(x; J, Z) = F_1 \cdot (X_J + iY_J) \cdot e^{i(\theta_J + Jx)}$$

where $X_J, Y_J \in \{+1, -1\}$ are the Hadamard-test measurement outcomes (real/imaginary parts) and $\theta_J = \arg(\hat{F}_j)$. This is an unbiased estimator of $\tilde{C}(x)$.

The variance is $\operatorname{Var}[G] \leq 2F_1^2$. For the smoothed step function $F$ with degree $d$ and transition width $\delta$: $|\hat{F}_j| = O(|j|^{-1})$, so $F_1 = O(\log d)$, giving $\operatorname{Var}[G] = O(\log^2 d)$.

To estimate $\tilde{C}(x)$ to accuracy $\sigma$: need $N_s = O(\log^2(d)/\sigma^2)$ samples. Expected total evolution time: $\tau \cdot N_s \cdot \mathbb{E}[|J|] = O(\tau d \log d / \sigma^2)$, which is $\tilde{O}(\tau d / \sigma^2)$.

## When to reach for it

- Estimating spectral functions (CDF, density, moments) from Hadamard-test data.
- Any setting where you want to evaluate a polynomial of eigenvalues but can only sample one Fourier component per circuit execution.
- When the Fourier coefficients decay (at least $O(1/|j|)$) — this is what keeps $F_1$ logarithmic.

## Why the $O(\log d)$ matters

If the Fourier coefficients were flat, $F_1 = O(d)$ and the variance would scale as $O(d^2)$ — no better than naive. The trick works because the smoothed step function has rapidly decaying Fourier coefficients. For sharper step functions (smaller $\delta$), $d$ grows but $F_1$ only grows logarithmically.

## Caveat

- Requires the specific decay structure of the Fourier coefficients. Doesn't work for arbitrary trigonometric polynomials with flat spectra.
- The estimator is complex-valued; you get both the real and imaginary parts of $\tilde{C}(x)$ from each sample. To recover just the real part with guaranteed accuracy, the variance bound still applies.
- Each sample requires a circuit with evolution time $|J|\tau$, which can be as large as $d\tau$ (the maximal evolution time). This is rare but possible.

## Related notes
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
- [[Approximate CDF from Hadamard Tests]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]]
