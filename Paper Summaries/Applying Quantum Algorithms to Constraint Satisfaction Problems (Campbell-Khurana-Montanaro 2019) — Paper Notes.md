> **Source:** Earl Campbell, Ankur Khurana, Ashley Montanaro, *Applying quantum algorithms to constraint satisfaction problems*, Quantum **3**:167, 2019
> **Links:** [arXiv](https://arxiv.org/abs/1810.05582) · [Journal](https://doi.org/10.22331/q-2019-07-18-167)
> **Tags:** #CSP #backtracking #Grover #SAT #graph-colouring #resource-estimation #fault-tolerant #surface-code

---

## The computational problem

Two prototypical NP-complete problems:

1. **$k$-SAT:** Given a boolean formula on $n$ variables in CNF with $m$ clauses of width $k$, find a satisfying assignment or prove none exists.
2. **$k$-colouring:** Given a graph $G$ on $n$ vertices (random Erdős-Rényi, edge probability 1/2), determine whether a proper $k$-colouring exists.

The paper asks: for random instances at or near the satisfiability/colourability threshold, how large a speedup can a fault-tolerant quantum computer achieve over the best classical solvers, within a one-day runtime budget?

---

## What the paper does

This is the practical follow-up to [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes|Montanaro's backtracking paper]]. It takes two general-purpose quantum algorithms — [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] and the [[Quantum Walk on Backtracking Trees via Effective Resistance|quantum backtracking walk]] — and compiles them down to concrete fault-tolerant circuits for random $k$-SAT and graph colouring. Every overhead is accounted for: T-depth, Toffoli count, magic state distillation, surface code parameters, classical decoding costs.

The headline result: speedup factors of $10^3$–$10^5$ relative to a classical desktop are possible in optimistic hardware scenarios, but the physical qubit counts are enormous ($> 10^{12}$) and the advantage disappears when you include the classical processing cost of surface code decoding.

My assessment: this paper is more about rigorous methodology for quantum advantage claims than about showing CSPs will be the first application. It sets the standard for how to do an honest quantum-vs-classical comparison with all overheads included.

---

## The algorithms

### Grover for $k$-SAT

The oracle checks whether an $n$-bit assignment satisfies all $m$ clauses. Key circuit optimisations:

- **Fan-out:** Each bit $x_i$ is fanned out to $m$ copies (or fewer via clause grouping) to enable parallel clause checking
- **Clause check:** Each clause is a Toffoli controlled on $k$ bits (testing whether the clause is violated)
- **AND of clauses:** A Toffoli controlled on $m$ bits, implemented as a depth-$\lceil\log m\rceil$ binary tree using Gidney's "uncompute AND" trick

Representative complexity for $k = 14$, $n = 78$: Toffoli depth **53**, Toffoli count $2.4 \times 10^7$ per Grover iteration.

Total Grover iterations: $3.642\sqrt{2^n}$ (Zalka's optimal bound for failure probability $\delta = 0.1$).

### Quantum backtracking for $k$-colouring and $k$-SAT

Uses the detection algorithm from [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes|Montanaro (2015)]]: phase estimation on $R_B R_A$ with precision $\beta/\sqrt{Tn}$. The paper optimises every component:

**Phase estimation:** Full QFT is overkill — only need to distinguish eigenvalue 1 from non-1. Replace the inverse QFT with Hadamard gates. Optimise $M = \sqrt{Tn}/b$ with $b = 1/32$, giving $K = 79$ repetitions for failure probability 0.1. Total controlled-$R_B R_A$ uses: $\leq 2528\sqrt{Tn}$ (all repetitions), $32\sqrt{Tn}$ (sequential depth).

**$R_A$ for $k$-colouring** (the heavy operation):

| Step | T-depth | T/Toffoli count |
|---|---|---|
| Conversion (input ↔ partial assignment) | 160 | $1.7 \times 10^7$ |
| Fan-out / fan-in | 2 | $1.4 \times 10^3$ |
| Compute $P(x)$, $h(x)$, colour count $c$ | 903 | $3.6 \times 10^6$ |
| Diffusion ($D$ or $D'$) | 1032 | $3.9 \times 10^3$ |
| Uncompute | 903 | $3.6 \times 10^6$ |
| **Total per $R_A$ step** | **3114** | **$2.5 \times 10^7$** |

For $k$-SAT the diffusion step dominates, since $P$ and $h$ are much simpler (checking clauses, incrementing a counter).

**Diffusion implementation:** Uses exact [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] (one iteration of Brassard-Høyer-Mosca-Tapp) to reflect about $|\psi\rangle = \frac{1}{\sqrt{c+2}}\sum_{i \in \{*\} \cup [c+1]} |i\rangle$ when $c + 1$ is not a power of 2. The root diffusion $D'$ (reflecting about $|\psi'\rangle = \alpha|*\rangle + \frac{\beta}{\sqrt{c+1}}\sum_{i \in [c+1]}|i\rangle$) uses an ancilla qubit + controlled-$D$ + Hadamard trick.

**$h(x)$ for DSATUR:** Find the most saturated vertex by computing $B_i$ = number of distinct colours adjacent to vertex $i$, then finding $\max_i B_i$ via a compare-and-swap tree. Uses Draper-Kutin-Rains-Svore adders for logarithmic-depth addition.

---

## Key results

### Speedup tables

**Grover for 14-SAT** (best $k$ for Grover):

| Regime | Max $n$ | T-depth | Toffoli count | Factory qubits | Speedup |
|---|---|---|---|---|---|
| Realistic | 65 | $1.46 \times 10^{12}$ | $4.41 \times 10^{17}$ | $3.14 \times 10^{13}$ | $1.62 \times 10^3$ |
| Plausible | 72 | $1.65 \times 10^{13}$ | $5.52 \times 10^{18}$ | $5.15 \times 10^{12}$ | $1.73 \times 10^4$ |
| Optimistic | 78 | $1.32 \times 10^{14}$ | $4.79 \times 10^{19}$ | $1.38 \times 10^{12}$ | $1.83 \times 10^5$ |

**Backtracking for graph colouring** (best performer for colouring):

| Regime | Max $n$ | T-depth | T/Toffoli count | Factory qubits | Speedup |
|---|---|---|---|---|---|
| Realistic | 113 | $1.70 \times 10^{12}$ | $8.24 \times 10^{17}$ | $6.29 \times 10^{13}$ | $7.25$ |
| Plausible | 128 | $1.53 \times 10^{13}$ | $9.94 \times 10^{18}$ | $9.26 \times 10^{12}$ | $5.17 \times 10^2$ |
| Optimistic | 144 | $1.62 \times 10^{14}$ | $1.24 \times 10^{20}$ | $3.59 \times 10^{12}$ | $4.16 \times 10^4$ |

### Classical benchmarks

- **$k$-SAT:** Compared against Maple LCM Dist (SAT Competition 2017 winner). Runtime scales as $2^{an+b}$ with $a \approx 0.03$–$0.56$ depending on $k$.
- **Graph colouring:** Compared against DSATUR (Brélaz 1979 + Sewell 1996). Tree size scales as $2^{0.40n - 7.43}$ (median). Each tree node $\approx$ 8000 CPU cycles on a 3.2 GHz i7.
- **Backtracking tree sizes for $k$-SAT:** Scale as $2^{f(n)}$ where $f(n) \approx 0.75n + 3.68$ for $k = 12$. Variables ordered by appearance count.

### Hardware parameters

| Parameter | Realistic | Plausible | Optimistic |
|---|---|---|---|
| 2-qubit gate time | 30 ns | 3 ns | 0.3 ns |
| Measurement time | 50 ns | 5 ns | 0.5 ns |
| Gate error rate | $10^{-3}$ | $10^{-4}$ | $10^{-5}$ |
| Surface code cycle time | 200 ns | 20 ns | 2 ns |

---

## Comparison: Grover vs. backtracking

A surprising finding: for the instance sizes solvable in one day, **Grover beats backtracking for $k$-SAT**. The quantum backtracking algorithm has lower asymptotic complexity ($\sqrt{T}$ instead of $2^{n/2}$, and $T < 2^n$ when backtracking prunes) but much larger constant overhead — the $R_A$ operation for SAT has T-depth $\sim 500$ vs. T-depth 53 for the Grover oracle. At $n \sim 70$, the overhead hasn't been amortised yet.

For graph colouring, Grover isn't competitive: $k^{n/2}$ is exponentially worse than $\sqrt{T}$ when DSATUR prunes heavily ($T \sim 2^{0.4n}$, $k \sim n/(2\log_2 n)$). The backtracking algorithm is the only viable option.

This is an important practical lesson: asymptotic superiority doesn't imply practical superiority for the problem sizes achievable on near-term fault-tolerant hardware.

---

## The classical processing overhead problem

Section 8 is the most sobering part. The quantum speedup disappears when you account for the classical processing required by the surface code decoder:

- Each qubit needs $\sim 1$ CPU for decoding at 200 ns cycle time
- With $10^{12}$+ factory qubits, that's $10^{12}$+ CPU-days of classical processing
- GPUs might reduce this by $100\times$, ASICs by $10^6\times$
- Even with ASICs, the classical decoding cost wipes out the quantum advantage for all regimes tested

This strongly motivates better error correction schemes with lower decoding overhead.

---

## Limits / caveats

1. **Random instances only.** Structured instances may be easier or harder classically; the comparison may not reflect practical use cases.

2. **Detection only, not search.** The quantum backtracking algorithm detects colourability but doesn't find a colouring. Finding costs an additional $O(n)$ factor (Jarret-Wan improved this to $\tilde{O}(\sqrt{Tn})$).

3. **Factory-dominated cost.** 95–99% of physical qubits are magic state factories, not logical qubits. This is the bottleneck.

4. **No state retention.** The quantum algorithm can't retain information between oracle calls, unlike classical DPLL which uses learned clauses and conflict-driven clause learning (CDCL). This gives modern SAT solvers a structural advantage not captured by the tree size.

5. **Surface code specificity.** Results depend heavily on the error correction scheme. A code with lower overhead (even at a worse threshold) might change the conclusions.

---

## Reusable ideas

1. [[Depth-Optimised Grover Oracle via Parallel Fan-Out]] — Fan out input bits to $m$ copies, check all clauses in parallel, AND via binary tree of Toffolis. Achieves Toffoli depth $O(\log k + \log m)$ at the cost of $O(mn)$ qubits. Applicable to any Grover oracle for a conjunction of local predicates.

2. [[Controlled-Swap Tree for Variable-Dependent Diffusion]] — When the diffusion operator $D_c$ depends on an input register $c$, precompute eigenvectors $|e_i\rangle$ of each $D_i$, then use a tree of controlled swaps (depth $\lceil\log k\rceil$) to route the working register to the correct eigenvector. General technique for parameter-dependent reflections.

---

## References within this paper

- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — the backtracking algorithm this paper compiles into circuits
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — the Grover search alternative
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|BBHT (1998)]] — tight analysis of Grover; Zalka's optimal iteration count
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — exact amplitude amplification used in diffusion
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015, Monte Carlo)]] — general-purpose quadratic speedup framework
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush-Gidney et al. (2018)]] — surface code compilation methodology, QROM
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders-Berry-Babbush et al. (2020)]] — similar resource estimation methodology for optimisation heuristics
- Ambainis-Kokainis, *Quantum algorithm for tree size estimation*, STOC 2017 — improved backtracking to handle the "lucky search" case
- Jarret-Wan, *Improved quantum backtracking algorithms*, PRA 97:022337 (2018) — search in $\tilde{O}(\sqrt{Tn})$ without uniqueness promise
- Gidney, *Halving the cost of quantum addition*, arXiv:1709.06648 (2017) — "uncompute AND" trick reducing Toffoli count
- Draper-Kutin-Rains-Svore, *A logarithmic-depth quantum carry-lookahead adder*, QIC 6:351 (2006) — depth-efficient addition circuits
- O'Gorman-Campbell, *Quantum computation with realistic magic-state factories*, PRA 95:032338 (2017) — magic state factory cost model
- Fowler-Mariantoni-Martinis-Cleland, *Surface codes*, PRA 86:032324 (2012) — surface code architecture

---

## Cross-links

### Paper notes
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]

### Trick cards
- [[Quantum Walk on Backtracking Trees via Effective Resistance]]
- [[Depth-Optimised Grover Oracle via Parallel Fan-Out]]
- [[Controlled-Swap Tree for Variable-Dependent Diffusion]]
