
> **Source:** Dominic W. Berry, Andrew M. Childs, Richard Cleve, Robin Kothari, and Rolando D. Somma, arXiv:1412.4687
> **Tags:** #trick #hamiltonian-simulation #taylor-series #segmentation

## What it does
Simulates $e^{-iHt}$ by truncating the Taylor series at order $K$ and choosing segment length so that the coefficient sum is exactly 2 (enabling [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]]).

## The trick
1. Divide time $t$ into $r = \lceil T / \ln 2 \rceil$ segments, where $T = |\alpha|_1 t$ ($\alpha$ = coefficients in $H = \sum \alpha_\ell H_\ell$)
2. Each segment has normalised time $\tau = \ln 2$
3. Truncate: $\tilde{U} = \sum_{k=0}^K \frac{(-i\tau)^k}{k!} H^k$
4. The coefficient sum $s = \sum_{k=0}^K \frac{(\ln 2)^k}{k!} \approx e^{\ln 2} = 2$ ✓

**Why $\ln 2$:** Makes $s = 2$ exactly (in the limit $K \to \infty$), which is the sweet spot for single-round OAA. The truncation error $|s - 2| \leq \varepsilon/r$ with:

$$K = O\!\left(\frac{\log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$$

This doubly-logarithmic scaling in $1/\varepsilon$ comes from Stirling: $(\ln 2)^k / k! \approx (e \ln 2 / k)^k$, super-exponentially small for $k \gg 1$.

## When to reach for it
- [[Hamiltonian simulation]] when you want near-optimal $\log(1/\varepsilon)$ precision dependence
- Any [[Linear Combination of Unitaries (LCU)|LCU]]-based simulation where you need the coefficient sum to be a specific constant
- As a cleaner alternative to Trotter-Suzuki for high-precision simulation

## Complexity
$r \cdot K = O\!\left(\frac{T \log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$ total uses of the Hamiltonian terms. Near-optimal in all parameters.

## Caveat
Each Taylor term $H^k = \sum_{\ell_1,\ldots,\ell_k} \alpha_{\ell_1}\cdots\alpha_{\ell_k} H_{\ell_1}\cdots H_{\ell_k}$ has $L^k$ terms — but [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] handles this via $K$ sequential controlled operations (not explicit enumeration). Ancilla cost is $O(K \log L)$ qubits.

## Related Paper Notes

- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — the source paper
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
