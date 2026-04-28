
> **Source:** Mária Kieferová, Daniel Scherer, and Dominic W. Berry, *Simulating the dynamics of time-dependent Hamiltonians with a truncated Dyson series*, arXiv:1805.00582, Phys. Rev. A **99**, 042314 (2019)  
> **Links:** [arXiv](https://arxiv.org/abs/1805.00582) · [PRA](https://doi.org/10.1103/PhysRevA.99.042314)  
> **Tags:** #dyson-series #LCU #time-dependent #hamiltonian-simulation

---

## The computational problem

Given oracle access to a time-dependent $d$-sparse Hamiltonian $H(\tau)$ on $n$ qubits, implement
$$
U(t_0,t_0+t)=\mathcal{T}\exp\!\left(-i\int_{t_0}^{t_0+t} H(\tau)\,d\tau\right)
$$
within error at most $\varepsilon$.

The input model is the standard sparse-Hamiltonian one: an oracle for the locations of nonzero entries and an oracle for the matrix elements of $H(\tau)$ at queried times. The goal is to get polylogarithmic dependence on $1/\varepsilon$, matching the truncated-[[Linear Combination of Unitaries (LCU)|LCU]] / Taylor-series story for time-independent simulation.

The difficulty is time ordering. The Dyson expansion contains integrals over simplices
$$
0\le t_1\le t_2\le \cdots \le t_k\le t,
$$
so a direct extension of the time-independent Taylor-series method needs a coherent way to prepare ordered tuples of times.

## What the paper does

Extends truncated-Taylor / LCU [[Hamiltonian simulation]] to the time-dependent setting by replacing the Taylor series with a truncated Dyson series. The underlying LCU structure is the same; the new work is the machinery for handling time-ordered integrals coherently.

The propagator is
$$
\mathcal{T} e^{-i\int_0^t H(s)\,ds},
$$
and each term in the Dyson expansion involves an integral over the simplex $t_1 \le t_2 \le \cdots \le t_k \le t$. The paper gives two concrete ways to realize those ordered tuples inside a coherent [[Linear Combination of Unitaries (LCU)|LCU]] circuit.

My read: this is a real conceptual step, not just a routine extension of truncated Taylor. Once you know the answer it looks natural, but the ordered-clock problem is the whole game here.

## The algorithm / construction

Write the evolution on a short segment of length $t/r$ as a truncated Dyson series,
$$
\mathcal{T}e^{-i\int_0^{t/r} H(\tau)\,d\tau}
\approx
\sum_{k=0}^{K}
(-i)^k
\int_{0\le t_1\le\cdots\le t_k\le t/r}
H(t_k)\cdots H(t_1)
\,dt_1\cdots dt_k.
$$

Then discretize the time integrals using $M$ time points and decompose each $H(\tau)$ into a linear combination of 1-sparse unitaries. This turns each segment into an [[Linear Combination of Unitaries (LCU)|LCU]] over products of time-labelled 1-sparse terms.

The segment circuit has the usual truncated-Taylor shape:

1. **Decompose $H(\tau)$.** Use sparse-Hamiltonian decomposition so that
   $$
   H(\tau)=\gamma\sum_{\ell=0}^{L-1} H_\ell(\tau),
   $$
   where each $H_\ell(\tau)$ is 1-sparse and unitary, and $L=\Theta(d^2 H_{\max}/\gamma)$.
2. **Discretize time.** Replace each simplex integral by a sum over ordered tuples of grid points.
3. **PREPARE.** Create the ancilla superposition over truncation order $k$, ordered time tuples, and LCU term labels $\ell_1,\dots,\ell_k$.
4. **SELECT.** Apply the ordered product
   $$
   H_{\ell_k}(t_k)\cdots H_{\ell_1}(t_1)
   $$
   controlled on the ancilla registers.
5. **Oblivious amplitude amplification.** Choose the segment size so the LCU 1-norm stays below 2, which lets one round of [[Oblivious Amplitude Amplification (Robust)]] make the procedure deterministic up to the target error.
6. **Repeat over $r$ segments.** Compose the short-time simulations to cover total time $t$.

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

## Key results

Let $H_{\max}=\max_\tau \|H(\tau)\|_{\max}$ and $\dot H_{\max}=\max_\tau \|\tfrac{d}{d\tau}H(\tau)\|$ in the paper's sparse-input model, and define the effective simulation parameter $\lambda=\Theta(d^2 H_{\max})$.

The headline result is that time-dependent sparse-Hamiltonian simulation can be done with query complexity nearly linear in $d^2 H_{\max} t$ and only polylogarithmic in $1/\varepsilon$.

More concretely, the paper obtains
$$
\text{queries}
=
O\!\left(
 d^2 H_{\max} t
 \frac{\log(d H_{\max} t/\varepsilon)}{\log\log(d H_{\max} t/\varepsilon)}
\right)
$$
up to the paper's suppressed polylog factors and oracle-cost conventions.

The truncation order and discretization scale as:

| Quantity | Value |
|---|---|
| Truncation order $K$ | $\Theta\!\left(\dfrac{\log(\lambda t/\varepsilon)}{\log\log(\lambda t/\varepsilon)}\right)$ |
| Number of segments $r$ | $\Theta(\lambda t)$ |
| Discretization $M$ (sort-based) | set by Riemann-sum error, hence derivative dependent |
| Discretization $M$ (gap-based) | larger than sort-based because repeated-time collisions must also be suppressed |

For the gate complexity, the main distinction is between the two clock constructions:

| Construction | Extra overhead per query block |
|---|---|
| Gap / compressed encoding | $O(\log M + n)$ |
| Sorting network | $O((\log M)(\log K) + n)$ |

The important structural point is this: the derivative dependence enters through the discretization of time, not through the leading query scaling in the same brutal way that older product-formula bounds did. That is what makes the method competitive.

## Comparison with prior work

| Method | Time dependence | Precision dependence | Main cost parameter | Bottleneck |
|---|---|---|---|---|
| [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes\|Wiebe-Berry-Høyer-Sanders]] | Smooth $H(t)$ | polynomial in $1/\varepsilon$ through step count | commutators + derivative bounds | product-formula error accumulation |
| [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes\|Poulin-Qarry-Somma-Verstraete]] | Arbitrary $H(t)$ | polynomial in $1/\varepsilon$ | $d^2\|H\|_{\max}t$ | randomized / averaged product formula |
| [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes\|Berry-Childs-Cleve-Kothari-Somma]] | Time-independent only | polylogarithmic in $1/\varepsilon$ | $d^2\|H\|_{\max}t$ | no time ordering needed |
| **This paper** | Time-dependent sparse $H(t)$ | polylogarithmic in $1/\varepsilon$ | $d^2 H_{\max} t$ | coherent ordered-time preparation |
| [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes\|Low-Wiebe]] | Time-dependent after frame change | polylogarithmic in $1/\varepsilon$ | norm of interaction-picture term | fast-forwardable split $H=A+B$ |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Berry-Childs-Su-Wang-Wiebe]] | Time-dependent | polylogarithmic in $1/\varepsilon$ for rescaled Dyson | $\int \|H(\tau)\|d\tau$ | time reparameterization |

The main improvement over the earlier time-dependent literature is not better asymptotic dependence on $d$ or $t$. It is restoring the truncated-Taylor style logarithmic precision dependence to the genuinely time-dependent case.

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

## Limits / caveats

- The discretization budget still depends on how fast $H(t)$ changes. If $\|\dot H\|$ is large, the time grid gets expensive.
- The gap-based construction is qubit-cheaper but pays an extra collision-analysis tax. The sort-based construction is cleaner but gate-heavier.
- The sparsity scaling is still quadratic in $d$ through the standard 1-sparse decomposition. This paper does not solve the later drive toward $d$-linear sparse simulation.
- This is not the end of the story for time-dependent simulation. [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe]] and [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes|Berry-Childs-Su-Wang-Wiebe]] improve the norm dependence in different directions.
- In my view, the paper is methodologically clean but not especially constant-friendly. If you cared about near-term implementation rather than asymptotics, the ancilla and control overhead would bite.

## Reusable ideas

1. [[Coherent Time-Ordering via Ancilla Clocks]] — realize Dyson-ordered integrals by preparing ordered tuples coherently inside an [[Linear Combination of Unitaries (LCU)|LCU]] ancilla.
2. [[Compressed Gap Encoding for Ordered Tuples]] — encode ordered times by their gaps to avoid sorting, trading circuit simplicity for collision bookkeeping.
3. [[Sorting-Based Time-Ordering for Dyson LCU]] — prepare unordered tuples and sort them reversibly when correctness for repeated times matters more than ancilla cost.
4. [[Collision Budgeting in Discretized Ordered Integrals]] — treat omitted repeated-time tuples as a birthday-paradox error term and budget $M$ accordingly.

## References within this paper

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] — [[Linear Combination of Unitaries (LCU)|LCU]] framework for implementing the truncated Dyson series
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — product-formula approach that this paper offers an alternative to
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer & Sanders (2010)]] — higher-order Suzuki [[Product Formulas]]s for time-dependent simulation; earlier foundational work establishing rigorous bounds for the product-formula alternative this paper supersedes
- Berry, Childs, Cleve, Kothari & Somma (2015) — Taylor-series LCU for time-independent simulation
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2019)]] — extends this work to the interaction picture
- [[Oblivious Amplitude Amplification (Robust)]] — used to boost LCU success probability
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — [[Standard Amplitude Amplification|amplitude amplification]] and [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]

---

## Cross-links

### Paper notes

- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — product-formula precursor for smooth time-dependent simulation
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]] — earlier end-to-end sparse time-dependent simulation with product formulas
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — derivative-free time-dependent simulation with weaker precision scaling
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — direct time-independent LCU ancestor
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — applies the same Dyson machinery after an $H=A+B$ split
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] — replaces $L^\infty$-in-time scaling by $L^1$ through time reparameterization
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]] — alternative line based on Magnus / commutator structure rather than explicit time ordering
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]] — reuses Dyson machinery for general linear ODE propagators
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]] — ports this paper's Dyson block-encoding ideas into a history-state ODE solver
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — Marika's later alternative route to high-precision simulation
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]] — earlier Kieferová paper with a similar coherent-LCU instinct
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]

### Trick cards

- [[Coherent Time-Ordering via Ancilla Clocks]]
- [[Compressed Gap Encoding for Ordered Tuples]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
- [[Collision Budgeting in Discretized Ordered Integrals]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Hamiltonian Simulation — Comparison Tables]]
