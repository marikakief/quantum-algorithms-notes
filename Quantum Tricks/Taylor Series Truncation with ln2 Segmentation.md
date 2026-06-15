
> **Source:** Dominic W. Berry, Andrew M. Childs, Richard Cleve, Robin Kothari, and Rolando D. Somma, arXiv:1412.4687
> **Tags:** #trick #hamiltonian-simulation #taylor-series #segmentation

## What it does
Simulates $e^{-iHt}$ by truncating the Taylor series at order $K$ and choosing segment lengths whose coefficient sums are close to the $s=2$ sweet spot for [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]].

## The trick
1. Divide time $t$ into $r = \lceil T / \ln 2 \rceil$ segments, where $T = |\alpha|_1 t$ ($\alpha$ = coefficients in $H = \sum \alpha_\ell H_\ell$)
2. Each segment has normalised time $\tau = T/r \leq \ln 2$; segments can be padded/diluted when exact $s=2$ normalization is needed
3. Truncate the normalized segment:
   $$
   \tilde{U} = \sum_{k=0}^K \frac{(-i\tau)^k}{k!}\bar H^k,\qquad \bar H=H/|\alpha|_1
   $$
4. The coefficient sum $s = \sum_{k=0}^K \frac{\tau^k}{k!}$ is at most $e^\tau \leq 2$ and is close to the infinite-series value when $K$ is large

**Why $\ln 2$:** The infinite Taylor coefficient sum is $e^\tau$, so $\tau=\ln 2$ gives $s=2$ in the infinite-series limit. That is the sweet spot for single-round OAA. For the truncated series and the final shorter segment, the algorithm uses truncation control and amplitude dilution/padding so the OAA condition is met to the required accuracy. The truncation error is controlled with:

$$K = O\!\left(\frac{\log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$$

This $\log/\log\log$ scaling comes from Stirling: $(\ln 2)^k / k! \approx (e \ln 2 / k)^k$, super-exponentially small for $k \gg 1$.

## When to reach for it
- [[Hamiltonian simulation]] when you want Taylor-LCU's near-optimal $\log(1/\varepsilon)/\log\log(1/\varepsilon)$ truncation order
- Any [[Linear Combination of Unitaries (LCU)|LCU]]-based simulation where you need the coefficient sum to be a specific constant
- As a cleaner alternative to Trotter-Suzuki for high-precision simulation

## Complexity
$r \cdot K = O\!\left(\frac{T \log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right)$ total uses of the Hamiltonian terms. This was near-optimal for Taylor-LCU simulation, but later QSP/qubitization improves the dependence to additive $O(T+\log(1/\varepsilon)/\log\log(1/\varepsilon))$ in the normalized query model.

## Caveat
Each Taylor term $H^k = \sum_{\ell_1,\ldots,\ell_k} \alpha_{\ell_1}\cdots\alpha_{\ell_k} H_{\ell_1}\cdots H_{\ell_k}$ has $L^k$ terms — but [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] handles this via $K$ sequential controlled operations (not explicit enumeration). Ancilla cost is $O(K \log L)$ qubits.

## Related Paper Notes

- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — the source paper
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
