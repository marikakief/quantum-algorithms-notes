_Comparison tables for time-dependent Hamiltonian simulation algorithms and classical dynamics simulation via quantum methods._

_Split from [[Hamiltonian Simulation — Comparison Tables]]._

---

## Time-Dependent Hamiltonian Simulation

### Abstract (input-model-agnostic) complexity: sparse and LCU models

| Algorithm | Year | SM gate complexity | LCU gate complexity | Norm parameter | Key technique |
|---|---|---|---|---|---|
| Monte Carlo ([[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes|Poulin-Qarry-Somma-Verstraete]]) | 2011 | $\widetilde{O}((d^2 \|H\|_{\max,\infty} t)^2 n/\varepsilon)$ | $O((\|\alpha\|_{1,\infty} t)^2 g_e/\varepsilon)$ | $L^\infty$ in time | Hamiltonian averaging |
| Fractional-query ([[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma 2015]]) | 2015 | $\widetilde{O}(d^2 \|H\|_{\max,1} n)$ queries | — | $L^1$ (queries only) | Fractional-query simulation |
| [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes\|Dyson series (Kieferová-Scherer-Berry 2018)]] | 2018 | $\widetilde{O}(d \|H\|_{\max,\infty} t\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,\infty} t\, L^2 g_c)$ | $L^\infty$ in time | Truncated Dyson + LCU |
| [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes\|Dyson/interaction picture (Low-Wiebe 2018)]] | 2018 | $\widetilde{O}(d \|H\|_{\max,\infty} t\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,\infty} t\, L^2 g_c)$ | $L^\infty$ (formal) | Interaction picture + Dyson |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Continuous qDRIFT (Berry-Childs-Su-Wang-Wiebe 2020)]] | 2020 | $\widetilde{O}((d^2 \|H\|_{\max,1})^2 n/\varepsilon)$ | $O(\|\alpha\|_{1,1}^2 g_e/\varepsilon)$ | **$L^1$ in time** | Norm-weighted time sampling |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Rescaled Dyson (Berry-Childs-Su-Wang-Wiebe 2020)]] | 2020 | $\widetilde{O}(d\,\|H\|_{\max,1}\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,1} L^2 g_c)$ | **$L^1$ in time** | Time reparameterization |
| [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes\|Vector-norm scaling (An-Fang-Liu 2021)]] | 2021 | Depends on $\|\psi(t)\|$ profile | — | Vector norm | Trotter + state-norm bounds |

**Notation:** $\|H\|_{\max,\infty} = \max_\tau \|H(\tau)\|_{\max}$; $\|H\|_{\max,1} = \int_0^t \|H(\tau)\|_{\max}\,d\tau$; $\|\alpha\|_{1,\infty} = \max_\tau \sum_l |\alpha_l(\tau)|$; $\|\alpha\|_{\infty,1} = \int_0^t \|\alpha(\tau)\|_\infty\,d\tau$; $\|\alpha\|_{1,1} = \int_0^t \sum_l |\alpha_l(\tau)|\,d\tau$. $g_c$ = cost of controlled-$H_l$; $g_e$ = cost of exact simulation of single term. $L$ = number of LCU terms.

The key insight of Berry-Childs-Su-Wang-Wiebe (2020): prior methods paid for $t\cdot\max_\tau\|H(\tau)\|$, scaling with peak norm over the full interval. Via [[Time-Reparameterization for Norm-Flattened Dyson Simulation|time reparameterization]], complexity reduces to the integrated norm $\int_0^t \|H(\tau)\|\,d\tau$. The improvement can be exponential when norm is concentrated on a short interval (e.g., reactive scattering).

## Classical Dynamics Simulation via Hamiltonian Simulation

### Quantum algorithms for classical differential equations

| Paper | Year | Problem | Encoding | Query complexity | Speedup | BQP-complete? |
|---|---|---|---|---|---|---|
| Costa, Jordan, Ostrander | 2019 | Wave equation (uniform, local) | Position-based ($\vec{y}$, $B^+\dot{\vec{y}}$) | $O(\text{poly}(N))$ (state prep dominated) | Polynomial | No |
| Jin, Liu, Yu | 2022 | General PDEs via Schrödingerisation | Various | Problem-dependent | Polynomial (generic) | No |
| An, Liu, Wang, Zhao | 2022 | General ODEs (theory of QDE solvers) | Standard ODE | Problem-dependent | Limited by fast-forwarding | No |
| [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes\|Berry, Costa]] | 2022 | Inhomogeneous linear ODEs ($\dot{x} = A(t)x + b(t)$) | History-state + Dyson block-encoding | $\tilde{O}(\lambda_A T \log(\lambda_{Ax}T/\varepsilon)\log(1/\varepsilon))$ calls to $U_A$ | Exponential (in $N$) | No |
| [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes\|Babbush, Berry, Kothari, Somma, Wiebe]] | 2023 | $2^n$ coupled harmonic oscillators | [[Energy-Balanced Encoding for Classical-to-Quantum Reduction\|Energy-balanced]] ($\sqrt{M}\dot{\vec{x}}$, $B^\dagger \vec{y}$) | $O(t\sqrt{\aleph d} + \log(1/\varepsilon))$ | **Exponential** | **Yes** |

The key difference between the last row and all others: the [[Energy-Balanced Encoding for Classical-to-Quantum Reduction|energy-balanced encoding]] makes the quantum state norm equal to the conserved total energy, eliminating condition-number bottlenecks from low-frequency modes. All other approaches encode $\vec{y}$ or $A\vec{y}$ directly, introducing $O(1/\omega_{\min})$ overhead.


---

**See also:** [[Quantum Chemistry — NISQ Experiments & VQE]] · [[Quantum Chemistry — Fault-Tolerant Resource Estimates]] · [[Quantum Chemistry — Circuit Primitives]] · [[Quantum Chemistry — Technique Crosswalk]]
