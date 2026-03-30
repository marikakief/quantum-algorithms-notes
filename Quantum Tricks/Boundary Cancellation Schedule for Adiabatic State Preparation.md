# Boundary Cancellation Schedule for Adiabatic State Preparation

> **Source:** Wecker, Hastings, Wiebe, Clark, Nayak, Troyer, arXiv:1506.05135; Lidar, Rezakhani, Hamma (2009); Wiebe, Babcock (2012)
> **Tags:** #trick #adiabatic #state-preparation #error-suppression #annealing-schedule

## What it does
Suppresses diabatic errors in adiabatic state preparation from $O(1/T)$ to $O(1/T^{m+1})$ by choosing smooth annealing schedules with vanishing derivatives at the endpoints.

## The trick
Instead of a linear interpolation $f(s) = s$, use:

$$f(s) = \frac{\int_0^s y^m (1-y)^m \, dy}{\int_0^1 y^m (1-y)^m \, dy}$$

This schedule satisfies $\partial^k_s f(s)\big|_{s=0,1} = 0$ for $k = 1, \ldots, m$ while still mapping $f(0) = 0, f(1) = 1$. The Hamiltonian path $H(f(s))$ then changes infinitely slowly at the boundaries.

The key bound (Wiebe-Babcock 2012): the diabatic error is at most

$$\frac{2 \max_s \|\partial_s^{m+1} H(f(s))\|}{\Delta(s)^{m+2} T^{m+1}}$$

For analytic Hamiltonians with a uniform gap, taking $m = O(\log(1/\delta))$ makes the total cost subpolynomial in $1/\delta$. Combined with Trotterised simulation (order $2k$ with $2k \approx m$), the full simulation cost is $O(\delta^{-2/m})$ — also subpolynomial.

The schedule moves slowly near the endpoints (where diabatic transitions from boundary effects dominate) and faster in the middle. This is complementary to local adiabatic interpolation (Roland-Cerf 2002), which slows down at the minimum gap.

## When to reach for it
- High-fidelity [[Adiabatic State Preparation for Chemistry|adiabatic state preparation]] where near-exact ground states are needed (e.g., for [[Non-Destructive Ground-State Measurement via Recovery Map|non-destructive measurement]])
- When the minimum gap occurs near the boundary of the adiabatic path (compatible with both boundary cancellation and local adiabatic methods)
- Preparing initial states for QPE where high overlap with the ground state is needed

## Complexity
- $O(T^{m+1})$ suppression of diabatic error
- Optimal $m$: $O(\log(1/\delta))$ for target error $\delta$
- Total Trotterised cost: $O(\delta^{-2/m})$ — subpolynomial for growing $m$

## Caveat
The smoothness requirement conflicts with local adiabatic interpolation when the minimum gap is far from the boundaries. In that case, forcing the schedule to be flat at the boundaries means it must change rapidly through the gap region. However, for many physical systems (including the Hubbard model), the gap minimum occurs near the end of the path, making the two strategies compatible.

Also, the theoretical scaling assumes the Hamiltonian is analytic in a strip about $[0,1]$. Non-analytic Hamiltonians (e.g., with sharp features) may not benefit from arbitrarily large $m$.

## Related notes
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]]
- [[Adiabatic State Preparation for Chemistry]]
- [[Phase-Diagram-by-Elimination via Adiabatic Gap Detection]]
- [[Boundary-Cancellation Schedule for Adiabatic Grover Search]]
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]]
