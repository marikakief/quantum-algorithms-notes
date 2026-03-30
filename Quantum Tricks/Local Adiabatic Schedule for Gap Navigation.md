# Local Adiabatic Schedule for Gap Navigation

> **Source:** van Dam, Mosca, Vazirani, arXiv:quant-ph/0206003; Roland-Cerf, arXiv:quant-ph/0107015
> **Tags:** #trick #adiabatic #spectral-gap #Grover #schedule

## What it does
Reduces adiabatic evolution time by adapting the interpolation rate to the local spectral gap, spending more time near narrow gaps and less time where the gap is large.

## The trick

In standard AQC with $H(s) = (1-s)H_0 + s H_f$, the constant-rate adiabatic condition requires total time $T \geq c \cdot \Delta_{\max}/g_{\min}^2$, where $g_{\min}$ is the minimum spectral gap over the entire path.

Instead, use a time-dependent schedule with delay $\tau(s) = c/g(s)^2$ at each instant $s$. The total evolution time becomes:

$$T = \int_0^1 \frac{c \, ds}{g(s)^2}$$

When the gap is small only in a narrow region around some $s^*$, this integral can be much smaller than $1/g_{\min}^2$.

**Example — unstructured search:** The gap profile is $g(s) = \sqrt{(2^n + 4(2^n-1)(s^2-s))/2^n}$ with $g_{\min} = 1/\sqrt{N}$ at $s = 1/2$. Constant rate gives $T = O(N)$. Local scheduling gives $T = \int_0^1 g(s)^{-2} \, ds = O(\sqrt{N})$.

## When to reach for it

- The spectral gap is concentrated near a point (e.g., avoided crossing between two levels)
- You have enough information about $g(s)$ to compute or bound the integral
- You want to match known query lower bounds (e.g., Grover's $\Omega(\sqrt{N})$) via an adiabatic algorithm

## Complexity

Total time $T = \int_0^1 g(s)^{-2} \, ds$, which can range from $O(1)$ (constant gap throughout) to the same $O(1/g_{\min}^2)$ as constant rate (if the gap is uniformly small).

## Caveat

Requires knowledge of $g(s)$ — or at least good bounds on it — which is generally intractable. For structured problems (search, some optimization) the gap can be computed analytically, but for generic AQC (e.g., random SAT instances) the gap is the unknown hard part.

[[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] proved this schedule is optimal for search: no choice of $s(t)$ can beat $O(\sqrt{N})$.

## Related notes
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]]
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]]
