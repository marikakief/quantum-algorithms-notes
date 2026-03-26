
> **Source:** Mária Kieferová, Daniel Scherer, and Dominic W. Berry, *Simulating the dynamics of time-dependent Hamiltonians with a truncated Dyson series*, arXiv:1805.00582, Phys. Rev. A **99**, 042314 (2019)  
> **Links:** [arXiv](https://arxiv.org/abs/1805.00582) · [PRA](https://doi.org/10.1103/PhysRevA.99.042314)  
> **Tags:** #dyson-series #LCU #time-dependent #hamiltonian-simulation

---

## What the paper does

Extends truncated-Taylor / LCU Hamiltonian simulation to the time-dependent setting by replacing the Taylor series with a truncated Dyson series. The underlying LCU structure is the same; the new work is the machinery for handling time-ordered integrals coherently.

The propagator is
$$
\mathcal{T} e^{-i\int_0^t H(s)\,ds},
$$
and each term in the Dyson expansion involves an integral over the simplex $t_1 \le t_2 \le \cdots \le t_k \le t$. The algorithmic challenge is implementing superpositions over these ordered tuples.

## Two approaches to time ordering

The paper gives two distinct constructions, with different tradeoffs.

### 1. Compressed-gap encoding (based on earlier work by Childs–Cleve–Jordan–Yonge-Mallo)
Encode the ordered tuple via its gaps $\Delta_j = t_{j+1} - t_j$ rather than the absolute times. Prepare this state using the exponential-gap construction of [Ref 21 in the paper], then recover absolute times by prefix sum.

- Advantage: smaller register, no sorting circuit needed
- Disadvantage: strictly increasing tuples only — repeated-time collisions are omitted and must be controlled via a birthday-type error analysis. This forces a larger discretization $M$.

### 2. Sort-based construction (quantum sorting networks)
Prepare an unordered superposition over all $k$-tuples of time indices, then apply a reversible sorting network (e.g. bitonic sort) with comparator outcomes stored in ancilla for uncomputation.

- Advantage: handles repeated times correctly, no collision analysis needed
- Disadvantage: sorting adds $O(K \log K)$ comparator gates and ancilla overhead

## Algorithm structure

Divide $[0,t]$ into $r = \Theta(\lambda t)$ segments. On each segment:

1. **PREPARE**: Load an ancilla state encoding time-tuple weights (plus LCU coefficients and ordering data via one of the two methods above).
2. **SELECT**: Apply the ordered product of 1-sparse Hamiltonian terms at the sampled times.
3. **Amplification**: Single-step oblivious amplitude amplification brings the success probability to 1 (possible because the LCU 1-norm is $\le 2$).

The Hamiltonian is decomposed into $L = \Theta(d^2 H_{\max}/\gamma)$ 1-sparse unitaries via the graph-coloring decomposition, with $\gamma = \Theta(\varepsilon/(d^3 t))$.

## Complexity

Let $\lambda = \Theta(d^2 H_{\max})$ where $d$ is sparsity and $H_{\max} = \max_\tau \|H(\tau)\|$.

| Quantity | Value |
|---|---|
| Truncation order $K$ | $\Theta\!\left(\log(\lambda t/\varepsilon)\cdot\log\log(\lambda t/\varepsilon)\right)$ |
| Discretization $M$ (compressed) | $\Theta\!\left(t\dot{H}_{\max}/(d\varepsilon H_{\max})\right)$, where $\dot{H}_{\max} = \max\|\dot H(\tau)\|$ |
| Query complexity | $O\!\left(d^2 H_{\max} t \cdot \log(d H_{\max} t/\varepsilon)\cdot\log\log(\cdots)\right)$ |
| Gates (sorted) | query count $\times\, O\!\left([\log M][\log K] + n\right)$ |
| Gates (compressed) | query count $\times\, O\!\left(\log M + n\right)$ (smaller const, larger $M$) |

The log-in-precision scaling (inherited from truncated Taylor) survives. The extra cost over the time-independent case comes from the ordering machinery and the derivative-dependent $M$.

## What is genuinely new

The algorithm is not hard to describe once you have the LCU toolkit — but the ordered-integral structure is the obstacle. The paper's contribution is showing two concrete ways to realize it, with careful error accounting for each.

The pattern generalizes: any algorithm needing coherent superposition over **ordered tuples** can use compressed-gap or sort-based encoding. This comes up beyond Dyson simulation (e.g., certain quantum chemistry state preparation circuits).

## Comparison table (conceptual)

| Aspect | Time-independent truncated Taylor | This paper |
|---|---|---|
| Series | Taylor | Dyson |
| Ordering needed? | No | Yes |
| Clock register | None | Yes (size $O(K \log M)$) |
| Precision scaling | $O(\log 1/\varepsilon)$ | $O(\log 1/\varepsilon)$ |

## Caveats

- Not the final word on simulation complexity. Later work (Low–Wiebe 2018, Berry et al. 2020) gets better parameter dependence by working in the interaction picture or scaling the time variable.
- The derivative-dependent $M$ is a real cost: if $\|dH/dt\|$ is large, $M$ can grow substantially.
- Gate counts have extra logarithmic factors compared to a simple statement of query complexity.

## References within this paper

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] — [[Linear Combination of Unitaries (LCU)|LCU]] framework for implementing the truncated Dyson series
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — product-formula approach that this paper offers an alternative to
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer & Sanders (2010)]] — higher-order Suzuki product formulas for time-dependent simulation; earlier foundational work establishing rigorous bounds for the product-formula alternative this paper supersedes
- Berry, Childs, Cleve, Kothari & Somma (2015) — Taylor-series LCU for time-independent simulation
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2019)]] — extends this work to the interaction picture
- [[Oblivious Amplitude Amplification (Robust)]] — used to boost LCU success probability
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]] and [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]

---

## Cross-links

- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — product-formula precursor; near-linear scaling for smooth $H(t)$ via optimized Suzuki order; direct predecessor in the time-dependent simulation lineage
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]] — companion algorithmic paper to the 2010 theory paper; full end-to-end product-formula simulation of time-dependent sparse Hamiltonians with oracle precision accounted for; this Dyson paper supersedes it in precision scaling
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — earlier polynomial-size simulation of arbitrary time-dependent Hamiltonians via generalised Trotter; this Dyson paper supersedes its efficiency
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Coherent Time-Ordering via Ancilla Clocks]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
- [[Hamiltonian Simulation — Comparison Tables]]
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]] — alternative approach avoiding time-ordering; matches first-order Dyson in worst case, beats it for commutator-structured problems
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]] — reuses the truncated Dyson series machinery for general (non-Hamiltonian) linear ODEs; adds USVA + compression gadget to fix the success-probability decay problem
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — applies this paper's Dyson block-encoding machinery to time-dependent ODEs (not just Hamiltonians); encodes propagators inside a history-state linear system
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — Marika's later Hamiltonian simulation paper; pursues randomized multi-product formulas as an alternative to the Dyson-LCU approach developed here
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]] — Marika's earlier paper; shares coherent-control + LCU philosophy, applied to adiabatic state preparation instead of real-time simulation
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]] — uses truncated Taylor series + history state for linear ODEs; this paper extends the approach to time-dependent Hamiltonians via Dyson series
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — improves Berry 2014 to exponential precision; shares the ODE-to-linear-system encoding that Berry-Costa 2022 then extends via this paper's Dyson machinery
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — the Dyson-LCU framework developed here is a component in several QLSP-based ODE solvers that build on this lineage
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]] — uses Berry-Costa 2022 (which builds on this paper) as the time-evolution subroutine for nonlinear ODEs after Carleman linearisation
