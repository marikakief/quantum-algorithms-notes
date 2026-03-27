> **Source:** Yuval R. Sanders, Dominic W. Berry, Pedro C. S. Costa, Louis W. Tessler, Nathan Wiebe, Craig Gidney, Hartmut Neven, and Ryan Babbush, *Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization*, arXiv:2007.07391, PRX Quantum **1**, 020312 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/2007.07391) · [PRX Quantum](https://journals.aps.org/prxquantum/abstract/10.1103/PRXQuantum.1.020312)
> **Tags:** #combinatorial-optimization #fault-tolerant #resource-estimation #QAOA #quantum-simulated-annealing #qubitization #surface-code #amplitude-amplification

---

## The computational problems

Four families of combinatorial optimization problems, chosen to span both practical programmable instances and theoretically interesting hard ensembles:

1. **$L$-term spin model**: $H_L = \sum_{\ell=1}^{L} w_\ell \prod_{i \in q_\ell} Z_i$, with $w_\ell \in \mathbb{R}$. The most general diagonal cost function considered.
2. **QUBO (Quadratic Unconstrained Binary Optimization)**: 2-local Ising model $H_{\text{QUBO}} = \sum_{i<j} J_{ij} Z_i Z_j + \sum_i h_i Z_i$, an NP-hard special case with $L = N(N+1)/2$.
3. **Sherrington-Kirkpatrick (SK) model**: QUBO with $w_{ij} \in \{-1, +1\}$ chosen randomly. A standard testbed for simulated annealing.
4. **Low Autocorrelation Binary Sequences (LABS)**: $H_{\text{LABS}} = \sum_{k=0}^{N-1} H_k^2$ where $H_k = \sum_{i=1}^{N-k} Z_i Z_{i+k}$. Has $O(N^3)$ terms but significant structure. Best classical algorithm scales as $\Theta(1.73^N)$.

The objective for each: find the bit string $x$ minimizing $E_x = \langle x | H | x \rangle$.

## What the paper does

This is a resource-estimation paper, not an algorithm paper. The authors compile optimized fault-tolerant circuits for the bottleneck primitives of six different quantum optimization heuristics, applied to the four problem families above. They report exact constant factors in Toffoli counts, ancilla requirements, and surface-code physical qubit/time budgets.

The main finding is sobering: **algorithms offering only quadratic speedup over classical optimization cannot achieve advantage on modest surface-code processors**. For the SK model at $N = 256$, quantum simulated annealing manages $\sim 3.3 \times 10^5$ walk steps per day on $\sim 8 \times 10^5$ physical qubits — equivalent to what classical simulated annealing does in about four CPU-minutes.

## The algorithms compiled

Six heuristic quantum optimization approaches are compiled:

| Algorithm | Primitive | Key Oracle |
|---|---|---|
| Amplitude amplification | Grover iterate on cost threshold | $O_{\text{direct}}$ (energy evaluation) |
| QAOA / Trotter steps | Phase by cost function + transverse field | $O_{\text{phase}}$ |
| Adiabatic / Hamiltonian walk | [[Qubitization (Quantum Walk for Spectral Encoding)\|Qubitized]] walk step | $O_{\text{LCU}}$ (PREPARE + SELECT) |
| Szegedy walk QSA | Szegedy walk step with controlled state prep | $O_{\text{diff}}$ + $O_{\text{fun}}$ |
| LHPST qubitized walk QSA | Single-transition qubitized walk step | $O_{\text{diff}}$ + $O_{\text{fun}}$ |
| Spectral gap amplified QSA | Walk step on amplified Hamiltonian | $O_{\text{diff}}$ + $O_{\text{fun}}$ |

A key structural insight: **most approaches are bottlenecked by the same oracle subroutines** (computing cost functions, phasing by cost, implementing LCU oracles). Improving those shared primitives benefits all algorithms.

## Oracle constructions (Section II)

Five oracle types are compiled for each of the four problems:

### Direct energy oracle $O_{\text{direct}}$

Computes $E_x$ into a register. Strategy: controlled addition/subtraction of each term's coefficient based on parity of the Pauli-$Z$ string.

| Problem | Toffoli count | Key idea |
|---|---|---|
| $L$-term | $L \cdot b_{\text{dir}}$ | Standard controlled addition |
| QUBO | $N^2 b_{\text{dir}} / 2 + O(N b_{\text{dir}})$ | No structure to exploit beyond 2-locality |
| SK | $< N^2$ | Sum of tree sums of bits — group $N(N-1)/2$ bits into $\lceil \log L \rceil$-sized blocks, sum each with tree sums, then add. Reduces from $N^2 \log N$ to $< 2L$ |
| LABS | $< 5N(N+1)/4$ | Exploits $H_k$ structure: compute partial sums, take absolute values, reuse tree sums; reduces $O(N^3)$ to $O(N^2)$ |

The SK and LABS oracles show how problem structure yields order-of-magnitude improvements. The LABS result is particularly nice: the naive $O(N^3)$ collapses to $O(N^2)$ by computing each $H_k$ separately and exploiting the sum-of-squares structure.

### Energy difference oracle $O_{\text{diff}}$

For QSA algorithms, computes $E_x - E_{x_k}$ where $x_k$ differs from $x$ by flipping bit $k$.

- **QUBO/SK**: Only $O(N)$ Toffolis because $\delta H^{(k)}$ is a 1-local operator after the flip.
- **LABS/$L$-term**: No cheap shortcut; requires two energy evaluations.

### Phase oracle $O_{\text{phase}}$

Phases computational basis by $e^{-i\gamma E_x}$. Two strategies depending on the problem:

- **$L$-term and QUBO**: Direct rotation synthesis per term, $O(L(b_{\text{pha}} + \log L))$ T gates (using [[Unary Iteration|unary iteration]] and repeat-until-success).
- **SK**: Compute energy → multiply by $\gamma$ into phase gradient state → uncompute. $2N^2 + b_{\text{pha}}^2/2 + O(b_{\text{pha}} \log b_{\text{pha}})$ Toffolis.
- **LABS**: Hybrid — compute partial sums and multiply into phase gradient state. $8N^2/5 + \min(Nb_{\text{pha}}^2/2, 9N^2/10) + O(Nb_{\text{pha}} \log b_{\text{pha}})$ Toffolis.

The **phase gradient trick** (addition into $|\phi\rangle = 2^{-b/2} \sum_\ell e^{-2\pi i \ell / 2^b} |\ell\rangle$) is used throughout. Addition into this state produces a phase kickback, converting multiplication-by-$\gamma$ into a sequence of additions.

### LCU oracles (PREPARE + SELECT)

For [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based walks. SELECT applies $Z_{q_\ell}$ controlled on $|\ell\rangle$ via [[Unary Iteration|unary iteration]]; PREPARE uses [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] via [[QROM (Quantum Read-Only Memory)|QROM]].

| Problem | Walk Toffoli count | $\lambda$ |
|---|---|---|
| $L$-term | $3L + 2b_{\text{LCU}} + O(\log L)$ | $\sum |w_\ell|$ |
| QUBO | $N(b_{\text{LCU}} + 2\log N) + O(N)$ | $\sum |w_\ell|$ |
| SK | $6N + O(\log^2 N)$ | $\approx N^2/2$ |
| LABS | $4N + O(\log N)$ | $\approx N^3/3$ |

The SK walk is remarkably cheap ($6N$ Toffolis) because the $\pm 1$ coefficients eliminate the need for coherent alias sampling — just a QROAM sign fixup. The LABS walk exploits the product structure of Eq. (85): SELECT applies four applications of [[Unary Iteration|unary iteration]] controlled on three registers $(i, j, k)$.

But $\lambda$ matters. For LABS, $\lambda \approx N^3/3$, so the walk must be repeated $O(N^3)$ times per unit of "Hamiltonian time." The per-step cheapness is offset by the number of steps.

### [[Adaptive QROM for Function Evaluation|QROM-based function evaluation]] $O_{\text{fun}}$

New technique for cheaply evaluating arithmetic functions (exponentials, arcsines) that arise in QSA transition probabilities. Key idea: **piecewise linear interpolation** between classically precomputed sample points accessed via QROM.

- Sample points are spaced at **exponentially growing intervals** (powers of 2), matching the behavior of exponential/arcsine functions. This uses a variable-spacing QROM variant.
- The number of interpolation regions scales as $O(2^{b_{\text{fun}}/2})$ with constant factors between 0.5 and 2 depending on the function.
- Dominant cost is the multiplication (slope × argument): $b_{\text{sm}}^2 + b_{\text{dif}} + O(b_{\text{sm}} \log b_{\text{sm}} + 2^{b_{\text{fun}}/2})$ Toffolis.
- For functions involving arcsine, the divergent slope near zero adds $b_{\text{fun}}$ extra bits, giving $(b_{\text{sm}} + b_{\text{fun}})^2 + O(\cdots)$.

Numerical simulation on 16-qubit SK instances confirms that very low precision (7-bit output, 0.01 angle error) has negligible impact on optimization performance. This is a useful general-purpose technique: whenever you need to evaluate a monotonic function of an oracle output inside a heuristic, you probably don't need high precision, and this QROM interpolation method is far cheaper than exact arithmetic.

## Algorithm compilations (Section III)

### Amplitude amplification (§III.A)

Used both standalone (Grover search over bit strings) and as a wrapper around other heuristics. Cost per step:

$$2C_{\text{direct}} + N + O(b_{\text{dir}})$$

Combined with exponential search (try $m = 2^j$ rounds for $j = 0, 1, 2, \ldots$), the mean cost to find a state in the target subspace $S$ scales as $O(C_{\text{direct}} / \sqrt{p_0})$ where $p_0$ is the initial overlap.

### QAOA (§III.B)

Each QAOA step requires one application of $O_{\text{phase}}$ plus a cheap transverse field rotation. Two new ideas for fault-tolerant QAOA energy estimation:

1. **Amplitude-estimation via Hadamard test**: estimate $\langle \psi | U_\ell | \psi \rangle$ for each term $\ell$, using phase estimation on the Grover iterate. Total cost $\sim 4p J \pi \lambda L C_{\text{phase}} / \Delta C$. Beats sampling when $\sigma^2 \geq 4\pi \lambda \Delta C L$.

2. **Amplitude-estimation via LCU**: combine the Hadamard test with PREPARE/SELECT to estimate $\langle C \rangle / \lambda$ directly. Cost $\sim 4\pi p J \lambda (C_{\text{phase}} + C_{\text{Sel}} + 2C_{\text{Prep}}) / \Delta C$. Quadratic improvement over sampling.

For heuristic QAOA (pre-optimized parameters, no outer loop), the cost per step simplifies to $C_{\text{phase}} + 4N + b_{\text{pha}}^2/2 + O(b_{\text{pha}} \log b_{\text{pha}})$ Toffolis.

### Adiabatic / Hamiltonian walk (§III.C)

Three variants:

**Trotter-based**: Standard [[Product Formulas]] discretization. Cost $\sim M \cdot C_{\text{phase}}$ for $M$ steps. Higher-order Suzuki formulas cost $2 \times 5^{\rho/2 - 1} \cdot M \cdot C_{\text{phase}}$ for order-$\rho$.

**Qubitized walk-based (new)**: Stroboscopically simulate the adiabatic path using qubitized walk steps. The walk at time $s$ encodes $\arccos(H(s)/\lambda)$; inflating $\lambda \to r\lambda$ shrinks each step to duration $\sim 1/r$, enabling refinement. The effective Hamiltonian generated is $r \arcsin(E_k/(r\lambda))$ per eigenvalue, which approaches $E_k/\lambda$ as $r \to \infty$. The convergence bound (Appendix B):

$$\text{Steps} \in \tilde{O}\left(\frac{1}{\varepsilon^{3/2}} \sqrt{\frac{\max_s(\|\ddot{H}\| + |\ddot{\lambda}|) \cdot \max_s(|\dot{\lambda}| + \|\dot{H}\|)}{\min(\Delta, \min_k |E_k|)^2} + \frac{\lambda \max_s(|\dot{\lambda}| + \|\dot{H}\|)^3}{\min(\Delta, \min_k |E_k|)^4}}\right)$$

The advantage over Trotter: cost per step doesn't scale with the number of Hamiltonian terms $L$. For LABS ($L = O(N^3)$), the walk step costs $4N + O(\log N)$ vs. $O(N^2)$ for Trotter.

**Zeno phase randomization**: Instead of measuring to project onto the ground state at each adiabatic step, evolve for a classically random time drawn from a distribution whose Fourier transform has width $< \Delta$ (the spectral gap). The authors optimize the window function and achieve $\langle |t| \rangle \Delta \approx 1.158$ with a 46th-order polynomial, improving over the $\text{sinc}^4$ distribution ($\langle |t| \rangle \Delta \approx 2.648$).

### Szegedy walk QSA (§III.D)

Direct implementation of the Somma-Boixo-Barnum-Knill quantum walk. Requires computing *all* $N$ transition probabilities $\Pr(x_k | x)$ for state preparation. The state preparation uses:

- Tree sums of bits (for computing energies)
- QROM-based function evaluation (for exponentials)
- A "prepare-from-squared-amplitudes" trick: prepare equal superposition over $z$, compare $z^2$ to $N \cdot \Pr(x_k | x)$, amplitude-amplify once

Cost per step: $\min(2(N+1)C_{\text{direct}}, 2NC_{\text{diff}}) + 2NC_{\text{fun}} + 2N\log N + 8Nb_{\text{sm}} + 18b_{\text{sm}}^2 + O(N)$.

This is the least efficient approach due to the $N$-fold overhead of computing all transition probabilities. The ancilla cost ($N \cdot A_{\text{diff}} + N \cdot A_{\text{fun}} + O(N)$) is also substantial.

### LHPST qubitized walk QSA (§III.E)

The Lemieux-Heim-Poulin-Svore-Troyer approach: same block-encoded operation as Szegedy, but only needs to compute *one* transition probability per step. The walk operator $\tilde{U}_W = R V^\dagger B^\dagger F B V$ decomposes into:

- $V$: equal superposition over $N$ bit-flip options
- $B$: controlled rotation by $\sqrt{p_{x,x_j}}$
- $F$: controlled bit flip of qubit $j$ in $x$
- $R$: reflection on zero

**This paper's key improvement**: Replace the LHPST rotation $B$ (which is exponential in connectivity $|N_j|$) with energy difference → QROM interpolation → phase-gradient rotation. This makes the method efficient for arbitrary-connectivity problems like SK and LABS, where LHPST's original approach would be exponential.

Cost per step for SK: $5N + 2(b_{\text{sm}} + b_{\text{fun}})^2 + 11\log N + O(b_{\text{sm}} \log b_{\text{sm}})$ Toffolis. Far cheaper than Szegedy.

### Spectral gap amplified QSA (§III.F)

Implements the Boixo-Ortiz-Somma Hamiltonian $\tilde{H}_\beta = A_\beta + \sqrt{\Delta_\beta}(\mathbb{1} - |0\rangle\langle 0|)$ whose spectral gap is the square root of the original Markov chain's gap.

The paper provides the **first explicit LCU oracle** for this Hamiltonian. The key construction: decompose $\sqrt{H_{\beta,k}}$ into a linear combination of unitaries via the integral representation $f = \int_0^1 (-1)^{z > 1 + f(x)} dz$, discretized with $2^s$ sample points.

The walk has $\lambda = (1 + 1/\sqrt{2})\sqrt{N}$. Cost per step is comparable to LHPST, but the $\sqrt{N}$ factor in $\lambda$ means more steps are needed, making this less efficient overall.

## Key results — resource estimates

### SK model at $N = 256$, surface code ($10^{-3}$ error rate, 1 Toffoli factory):

| Algorithm | Toffoli/step | Steps/day | Physical qubits |
|---|---|---|---|
| Amplitude amplification | $\sim 10^5$ | $4.8 \times 10^3$ | $8.1 \times 10^5$ |
| QAOA / 1st-order Trotter | $\sim 10^5$ | $4.7 \times 10^3$ | $8.6 \times 10^5$ |
| Hamiltonian walk | $\sim 1.5 \times 10^3$ | $3.3 \times 10^5$ | $8.0 \times 10^5$ |
| LHPST walk QSA | $\sim 1.5 \times 10^3$ | $3.3 \times 10^5$ | $8.4 \times 10^5$ |
| Gap amplified QSA | $\sim 1.3 \times 10^3$ | $3.9 \times 10^5$ | $8.4 \times 10^5$ |

### The bottom line

For $N = 512$ SK, classical SA (using Isakov et al.'s optimized code) performs $\sim 6 \times 10^{11}$ updates per core-hour vs. $\sim 8 \times 10^3$ quantum walk steps per hour. Even assuming an *exactly quadratic* speedup in the number of steps, the quantum computer would need $\sim 7 \times 10^7$ steps to break even — requiring about **one year** of continuous quantum computation. And that's generous: it compares to a *single classical core*.

For LABS at $N = 64$, comparing quantum amplitude amplification ($\sim 2 \times 10^3$ steps/hour) to the $\Theta(1.73^N)$ branch-and-bound algorithm ($\sim 5 \times 10^8$ steps/hour), the crossover requires $\sim 2 \times 10^9$ quantum queries, taking about **116 years**.

## Comparison with prior work

| Aspect | This paper | Prior state of the art |
|---|---|---|
| QSA circuit compilation | First constant-factor analysis for all three variants | Somma et al. (2008), LHPST (2020): asymptotic only |
| LHPST rotation for dense Hamiltonians | $O(C_{\text{diff}} + C_{\text{fun}})$ | LHPST original: $O(N 2^{|N_j|})$ — exponential in connectivity |
| Spectral gap amplified walk | First explicit LCU oracle | Boixo, Ortiz, Somma (2015): theoretical construction only |
| QAOA energy estimation | Amplitude estimation via LCU gives quadratic improvement | Farhi, Goldstone, Gutmann (2014): direct sampling |
| Adiabatic via qubitized walk | New: stroboscopic walk approach | Wan & Kim (2020): Dyson series approach |
| QROM function evaluation | New: piecewise-linear interpolation at exponential spacings | Exact arithmetic circuits |

## Limits / caveats

1. **The resource estimates assume a single Toffoli factory.** Parallelizing state distillation with tens of millions of qubits helps but hits Clifford bottlenecks — at best 100–1000× speedup, not enough to close the gap.

2. **No claim about algorithm performance.** This paper deliberately avoids the question of *how many steps each heuristic needs*. The resource estimates measure the cost of a single primitive. Whether quantum optimization heuristics outperform classical ones on any ensemble remains open.

3. **Quadratic speedup is the optimistic assumption.** For QSA, the $O(1/\sqrt{\Delta})$ vs. $O(1/\Delta)$ comparison uses the worst-case classical bound, which is very loose for practical simulated annealing.

4. **The walk steps are not directly comparable.** Hamiltonian walk steps simulate time $\sim 1/\lambda$, while QSA steps make one Markov-chain-like update. A Hamiltonian walk step for LABS ($\lambda \approx N^3/3$) is cheap but tiny.

5. **Marika's boundary cancellation methods** (Kieferová & Wiebe 2014, cited as [55]) are invoked to improve the adiabatic scaling to $\tilde{O}(\log^{1+o(1)}(1/\varepsilon) / \Delta)$ when $\Delta^2 \gg \varepsilon$. This helps the rigorous adiabatic algorithm but doesn't change the heuristic picture.

## My assessment

This paper is the definitive "reality check" for fault-tolerant quantum optimization. It's thorough (79 pages, constant factors everywhere), and the conclusion is stark: the surface-code overhead is so large that quadratic quantum speedups get eaten alive. The comparison to Isakov's classical SA code is especially damning — $10^8 \times$ more classical updates per hour than quantum walk steps.

The technical contributions are genuinely useful beyond the pessimistic conclusion. The [[Adaptive QROM for Function Evaluation|QROM interpolation method]] for cheap function evaluation, the improved LHPST rotation for dense Hamiltonians, and the explicit spectral gap amplification oracle are all reusable building blocks. The observation that most optimization algorithms share the same oracle bottlenecks is a helpful organizing principle.

For Marika: Yuval is first author on this, and your boundary cancellation work shows up in the adiabatic analysis (Section III.C). The paper also cites Berry, [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová et al. (2018)]] for qubitization conventions. If you're ever asked "why not just run QAOA on a fault-tolerant machine?", this paper is the quantitative answer.

The open question it leaves: is there a quantum optimization heuristic with *better than quadratic* speedup on any structured ensemble? The paper strongly motivates that search.

## Reusable ideas

1. [[Adaptive QROM for Function Evaluation]] — Piecewise linear interpolation via variable-spacing QROM for cheap evaluation of arithmetic functions inside quantum circuits.
2. [[Phase Gradient State for Controlled Rotations]] — Reusable ancilla state $|\phi\rangle$ that converts additions into phase rotations via kickback; enables $b-2$ Toffoli rotations from $b$-bit angles.
3. [[Sum of Tree Sums for Bit Counting]] — Grouping bits into blocks of $\lceil \log L \rceil$, tree-summing each, adding into a running total; reduces Toffoli cost of summing $L$ bits from $L \log L$ to $< 2L$ with $O(\log L)$ ancilla.
4. [[Stroboscopic Adiabatic Walk]] — Using inflated-$\lambda$ qubitized walks ($\lambda \to r\lambda$) to approximate short-time adiabatic evolution; cost per step independent of number of Hamiltonian terms.
5. [[LHPST Rotation via Energy Difference Oracle]] — Replacing the exponential-in-connectivity LHPST rotation with energy difference computation + QROM function evaluation; makes qubitized QSA efficient for arbitrary-connectivity problems.

## References within this paper

Key papers cited (with vault links where available):

- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Source of [[Unary Iteration|unary iteration]], [[QROM (Quantum Read-Only Memory)|QROM]], and [[Coherent Alias Sampling for PREPARE|coherent alias sampling]]
- [[Qubitization (Quantum Walk for Spectral Encoding)|Low & Chuang (2019)]] — Qubitization framework
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush (2019)]] — [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] (dirty ancilla QROM) and [[Measurement-Based QROM Uncomputation|measurement-based uncomputation]], used for PREPARE on QUBO
- Somma, Boixo, Barnum, Knill (2008) — Original quantum simulated annealing via Szegedy walks
- Lemieux, Heim, Poulin, Svore, Troyer (2020) — LHPST qubitized QSA approach
- Boixo, Ortiz, Somma (2015) — Spectral gap amplification construction
- Farhi, Goldstone, Gutmann (2014) — QAOA
- Gidney (2018) — Addition circuits, phase gradient trick
- Gidney & Fowler (2019) — Surface code compilation and Toffoli state distillation costs
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes|Kieferová & Wiebe (2014)]] — Boundary cancellation methods for improved adiabatic scaling
- Berry, Kieferová, Scherer, Sanders, Low, Wiebe, Gidney, Babbush (2018) — Qubitization conventions; Sanders is also a co-author there
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — Szegedy quantum walk framework
- Boixo, Knill, Somma (2009) — Zeno phase randomization approach
- Isakov, Zintchenko, Rønnow, Troyer (2015) — The classical SA code used for comparison

## Cross-links

### Paper notes
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Source of QROM, unary iteration, coherent alias sampling; this paper extends those tools to optimization
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — Also uses QROM + coherent alias sampling for PREPARE; THC qubitization shares the same LCU query model
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — First-quantized qubitization shares oracle design patterns; also provides surface-code resource estimates
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — [[Asymmetric Qubitization|Asymmetric qubitization]] for non-optimization Hamiltonians; shares qubitized walk framework
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — Also compiles interaction-picture approaches with detailed resource estimates
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — Same author group makes the [[Stroboscopic Adiabatic Walk]] rigorous and optimal for the QLSP via the [[Discrete Adiabatic Theorem for Quantum Walks]]
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Uses the [[Sum of Tree Sums for Bit Counting|bit-summing]] technique from this paper for [[Clique Checking via Edge Counting|clique checking]] in the TDA algorithm; also uses the equal-superposition preparation method
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Uses the QROM function interpolation technique from this paper as the first stage of a hybrid QROM+Newton-Raphson inverse square root computation for [[Product Formulas]] simulation
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]] — A fundamentally different approach to quantum optimization: reduces to syndrome decoding rather than Hamiltonian-based methods; achieves superpolynomial speedup for OPI where this paper's compiled heuristics offer at best quadratic advantages
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — contrasts with this paper's pessimism: for coupled oscillators a genuine exponential speedup exists, unlike the at-most-quadratic advantage found here for combinatorial optimization heuristics
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — a similar "reality check" paper for quantum chemistry advantage; both papers temper optimism by carefully accounting for constant factors and classical comparison points
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry, Rubin, Babbush et al 2024]]
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes|Costa, An, Babbush, Berry 2023]]

### Trick cards
- [[QROM (Quantum Read-Only Memory)]] — Used throughout for oracle construction
- [[Unary Iteration]] — Used for SELECT in LCU oracles and controlled bit flips
- [[Coherent Alias Sampling for PREPARE]] — Used for PREPARE in qubitized walks
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — Walk framework for Hamiltonian, LHPST, and gap-amplified approaches
- [[Adaptive QROM for Function Evaluation]] — New trick from this paper
- [[Phase Gradient State for Controlled Rotations]] — New trick from this paper
- [[Sum of Tree Sums for Bit Counting]] — New trick from this paper
- [[Stroboscopic Adiabatic Walk]] — New trick from this paper
- [[LHPST Rotation via Energy Difference Oracle]] — New trick from this paper
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]] — Qualtran implements this paper's variable-spaced QROM, QVR bloqs, and Hamming weight phasing as standard library primitives
