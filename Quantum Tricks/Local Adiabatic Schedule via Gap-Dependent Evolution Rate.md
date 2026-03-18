# Local Adiabatic Schedule via Gap-Dependent Evolution Rate

> **Source:** Roland & Cerf, Phys. Rev. A 65, 042308 (2002); arXiv:quant-ph/0107015
> **Tags:** #trick #adiabatic #search #schedule-optimization

## What it does
Recovers optimal $O(\sqrt{N})$ running time for adiabatic search by varying the interpolation speed according to the instantaneous spectral gap — slow through the bottleneck, fast elsewhere.

## The trick

Standard adiabatic computation interpolates $H(s) = (1-s)H_0 + sH_m$ at constant rate $s = t/T$. The adiabatic condition $|\dot{s}| \cdot |\langle dH/ds \rangle_{1,0}| / g^2 \leq \varepsilon$ must hold everywhere, so the total time is set by the worst-case (minimum) gap: $T \sim 1/g_{\min}^2$.

The fix: **enforce adiabaticity locally** at each instant by setting

$$\frac{ds}{dt} = \varepsilon \cdot g^2(s)$$

This makes the schedule an S-curve: the system races through regions of large gap and crawls through the gap minimum. The total time integrates to

$$T = \int_0^1 \frac{ds}{\varepsilon \cdot g^2(s)}$$

For unstructured search with $g_{\min} = 1/\sqrt{N}$ at $s = 1/2$, this gives $T = \frac{\pi}{2\varepsilon}\sqrt{N}$ instead of $O(N)$.

**Why it works:** The constant-rate approach wastes time evolving slowly through regions where the gap is large and the system wouldn't leave the ground state anyway. The local schedule allocates time proportionally to where it's actually needed.

## When to reach for it
- Any adiabatic algorithm where the gap varies significantly along the path
- When you know the gap profile $g(s)$ (analytically or numerically)
- Designing quantum annealing schedules with non-uniform sweep rates

## Complexity
Total time $T = \int_0^1 ds / (\varepsilon \cdot g^2(s))$. For search: $O(\sqrt{N})$.

## Caveat
Requires knowledge of $g(s)$, which is generally hard for NP-hard problems (knowing the gap is essentially knowing the answer). Works beautifully when the gap is analytically tractable. For problems where the gap is unknown, heuristic or adaptive schedules are needed.

## Related notes
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Adiabatic State Preparation for Chemistry]]
