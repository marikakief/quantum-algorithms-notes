> **Source:** Jérémie Roland and Nicolas J. Cerf, *Quantum Search by Local Adiabatic Evolution*, Phys. Rev. A **65**, 042308 (2002); arXiv:quant-ph/0107015
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0107015)
> **Tags:** #quantum-algorithm #adiabatic #search #Grover #lower-bound

---

## The problem

Farhi et al. (2000) proposed adiabatic quantum computation: interpolate $H(t) = (1-s)H_0 + s H_m$ from an easy initial Hamiltonian to one whose ground state encodes the solution. Applied to unstructured search, their constant-rate interpolation requires time $T = O(N)$ — no speedup over classical.

Can you do better by varying the interpolation rate?

## What the paper does

Yes. By adjusting $ds/dt$ locally to satisfy the adiabatic condition at each instant (not just globally), the total evolution time drops to $T = \frac{\pi}{2\varepsilon}\sqrt{N}$ — recovering the Grover $O(\sqrt{N})$ speedup. The paper also proves this is optimal: no choice of $s(t)$ can beat $O(\sqrt{N})$.

Four pages. Clean and complete.

---

## Setup

$N = 2^n$ items, marked state $|m\rangle$. Define:

$$H_0 = I - |\psi_0\rangle\langle\psi_0|, \quad H_m = I - |m\rangle\langle m|$$

where $|\psi_0\rangle = \frac{1}{\sqrt{N}}\sum_i |i\rangle$. Ground states are $|\psi_0\rangle$ and $|m\rangle$ respectively (eigenvalue 0).

Interpolating Hamiltonian: $\tilde{H}(s) = (1-s)H_0 + s H_m$ with $s = s(t)$ going from 0 to 1.

The spectrum: one $(N-2)$-degenerate eigenvalue at $E_2 = 1$, and two non-degenerate eigenvalues $E_0, E_1$ with gap:

$$g(s) = \sqrt{1 - \frac{4(N-1)}{N}s(1-s)}$$

Minimum gap $g_{\min} = 1/\sqrt{N}$ at $s = 1/2$.

---

## Global vs. local adiabatic condition

**Global (Farhi et al.):** Require $\frac{|\langle dH/dt \rangle_{1,0}|}{g_{\min}^2} \leq \varepsilon$ over the entire evolution. Since $g_{\min} = 1/\sqrt{N}$, this gives $T \geq N/\varepsilon$. No quantum advantage.

**Local (this paper):** Require $\left|\frac{ds}{dt}\right| \leq \varepsilon \cdot g^2(s)$ at each instant. Evolve slowly when the gap is small (near $s = 1/2$), fast when it's large. This gives:

$$\frac{ds}{dt} = \varepsilon\left(1 - \frac{4(N-1)}{N}s(1-s)\right)$$

Integrating:

$$t(s) = \frac{1}{2\varepsilon}\frac{N}{\sqrt{N-1}}\left[\arctan\left(\sqrt{N-1}(2s-1)\right) + \arctan\sqrt{N-1}\right]$$

Setting $s = 1$ and taking $N \gg 1$: $T = \frac{\pi}{2\varepsilon}\sqrt{N}$. (The exact finite-$N$ expression is $T = \frac{1}{\varepsilon}\frac{N}{\sqrt{N-1}}\arctan\sqrt{N-1}$, which converges to the $\pi/2$ form as $N \to \infty$.)

The schedule $s(t)$ is an S-curve: fast at the endpoints, slow through the bottleneck at $s = 1/2$ where the gap closes. The slowdown is exactly calibrated so the integrated evolution time is $O(\sqrt{N})$.

---

## Optimality proof

Uses the same framework as the Farhi-Gutmann continuous-time search lower bound. For any evolution schedule $s(t)$:

1. Final states $|\psi_m, T\rangle$ for different marked items $m$ must become distinguishable (nearly orthogonal) after time $T$
2. The rate at which $\langle\psi_m|\psi_{m'}\rangle$ can decrease is bounded by the $m$-dependent part of the Hamiltonian: $\|H_m|\psi\rangle\| \leq s(t)$
3. Integrating this bound over the evolution and summing over pairs gives $T = \Omega(\sqrt{N})$

So $O(\sqrt{N})$ is tight. No schedule does better.

---

## Key results

| Result | Statement |
|---|---|
| Local adiabatic search time | $T = \frac{\pi}{2\varepsilon}\sqrt{N}$ |
| Global adiabatic search time | $T = O(N)$ (no speedup) |
| Optimality | No evolution schedule $s(t)$ achieves $o(\sqrt{N})$ |
| Gap structure | $g_{\min} = 1/\sqrt{N}$ at $s = 1/2$; the local schedule compensates for this |

---

## Why it matters

This paper bridges adiabatic and gate-model quantum computation for search. It shows that the "adiabatic algorithms are slow" intuition from Farhi et al.'s $O(N)$ result was an artefact of using a constant evolution rate, not a fundamental limitation.

The local adiabatic idea — slow down where the gap is small — is the template for all subsequent adiabatic algorithm design. It also connects to:

- **Quantum annealing:** practical schedules inspired by gap-aware evolution rates
- **Adiabatic-to-circuit equivalence:** Roland-Cerf is one of the key examples showing AQC can match gate-model efficiency
- The subsequent proof that AQC is polynomially equivalent to gate-model QC (Aharonov et al. 2007) builds on understanding exactly this kind of schedule optimisation

---

## Limits / caveats

- The optimal schedule requires knowing the gap $g(s)$ analytically, which depends on the problem structure. For unstructured search, the gap is known; for NP-hard problems, it generally isn't.
- The paper suggests applying local adiabatic evolution to NP-complete problems, but this hasn't led to proven speedups beyond the search case.
- The result is specific to the interpolation $H(s) = (1-s)H_0 + sH_m$ with $H_0 = I - |\psi_0\rangle\langle\psi_0|$ and $H_m = I - |m\rangle\langle m|$. Different Hamiltonians could have different gap structures.

---

## Reusable ideas

1. [[Local Adiabatic Schedule via Gap-Dependent Evolution Rate]] — slow down $ds/dt \propto g^2(s)$ through the gap bottleneck to achieve optimal total evolution time

---

## Cross-links

### Paper notes
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the gate-model version of the same problem
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]] — another application of Grover search
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]] — the constant-rate predecessor; this paper fixes it
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]] — generalises the local adiabatic idea to QLSP with power-law and smooth schedules
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]] — uses the search Hamiltonian as a test case; shows the $O(1/T)$ boundary term vanishes at specific $T$ values (Eq. 71)

### Trick cards
- [[Local Adiabatic Schedule via Gap-Dependent Evolution Rate]]
- [[Gap-Adapted Adiabatic Scheduling]] — generalised version of the local adiabatic idea with tunable exponent $p$
- [[Phase Kickback from Oracle Queries]] — the oracle mechanism underlying both adiabatic and gate-model search

### Follow-up
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — Discretises the local adiabatic schedule with Trotter step size 1; generalises to $1 \leq p < 2$; adds boundary-cancellation variant achieving super-polynomial precision
