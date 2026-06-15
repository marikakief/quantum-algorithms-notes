> **Source:** Thomas Häner, Martin Roetteler, Krysta M. Svore, *Optimizing Quantum Circuits for Arithmetic*, arXiv:1805.12445 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1805.12445)
> **Tags:** #quantum-arithmetic #function-evaluation #piecewise-polynomial #Newton-method #reversible-computation #circuit-primitive #arcsine #inverse-square-root

---

## The computational problem

Given a classical function $f(x)$ (e.g., $\tanh$, $e^{-x^2}$, $\sin$, $\cos$, $1/\sqrt{x}$, $\arcsin$, $e^{-x}$), build a reversible quantum circuit that evaluates $f$ on an $n$-bit fixed-point register in superposition:

$$|x\rangle|0\rangle \mapsto |x\rangle|f(x)\rangle$$

The cost measures are Toffoli count, qubit count, and circuit depth. The target precision is an $L_\infty$ error $\varepsilon$ over a specified domain $\Omega$.

This is a subroutine problem: many quantum algorithms need to evaluate classical functions inside oracles. Examples include [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] (needs $1/x$ and $\arcsin$), quantum chemistry (needs $1/\sqrt{x}$ for Coulomb integrals and Gaussians for basis functions), quantum machine learning (sigmoid/tanh activations), and [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|quantum Metropolis sampling]] / quantum rejection sampling (needs $\arcsin$).

## What the paper does

Provides concrete, gate-level reversible circuits for evaluating a library of common mathematical functions, along with a software-stack module that automatically generates circuits for arbitrary piecewise smooth functions. The two main techniques are:

1. **Parallel piecewise polynomial evaluation** — partition the domain into subintervals, fit a low-degree minimax polynomial on each, and coherently evaluate the polynomial selected by a label register. The circuit uses controlled coefficient selection; it does not write all $M$ polynomial values into separate output registers. This handles smooth functions ($\tanh$, Gaussian, $\sin$, $\cos$, $e^{-x}$).

2. **Reversible Newton-Raphson iteration for $1/\sqrt{x}$** — adapted from the "fast inverse square root" trick in Quake III Arena. A bit-level initial guess (from the position of the leading 1-bit) feeds into Newton iterations $x_{n+1} = x_n(1.5 - ax_n^2/2)$. This is then composed with polynomial evaluation to implement $\arcsin(x)$ via the Cephes double-angle identity.

All circuits are implemented at the Toffoli gate level in LIQUi$|\rangle$ and tested on a reversible simulator with registers up to several hundred qubits.

## The algorithm / construction

### Parallel piecewise polynomial evaluation

**Problem:** A single polynomial of degree $d$ may need very high $d$ to approximate $f$ over all of $\Omega$ to error $\varepsilon$. Piecewise approximation uses $M$ subintervals with low-degree polynomials on each.

**Naive approach:** Loop over subintervals and conditionally evaluate the matching polynomial. Cost grows as $O(M)$.

**Their approach:** Use a label register $|l\rangle$ of $\lceil \log_2 M \rceil$ qubits to evaluate the selected polynomial coherently, with controlled coefficient loads providing the "parallel" dependence on the interval:

1. **Label computation.** Compare $x$ against $M$ interval boundaries using the CARRY circuit from [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner-Roetteler-Svore (2017)]]. Each comparison costs $2n$ Toffolis. The label register is set to indicate which subinterval $\Omega_l$ the input belongs to. Total: $4Mn$ Toffolis.

2. **Parallel Horner evaluation.** At each Horner iteration, a NEXTa gate swaps in the correct coefficient $a_{l,i}$ conditioned on $|l\rangle$. This is a $\lceil \log_2 M \rceil$-controlled NOT operation, implementable with $4(\lceil \log_2 M \rceil - 2)$ Toffolis and dirty ancillae (from Barenco et al. (1995)). The multiplication and addition proceed identically to single-polynomial Horner; the overhead from parallelism is just the controlled coefficient loads.

3. **Pebble-game optimization.** The $d+1$ intermediate Horner iterates can be traded against depth using [[Pebble Game Space Reduction for Recursive Circuits|Bennett pebbling]] on the linear dependency graph. Table I in the paper gives optimal pebbling strategies for all $(m, r)$ combinations.

**Toffoli count for parallel polynomial evaluation:**

$$T_{\text{pp}}(n, d, p, M) = \tfrac{3}{2}n^2 d + 3npd + \tfrac{7}{2}nd - 3p^2 d + 3pd - d + 2Md(4\lceil \log_2 M \rceil - 8) + 4Mn$$

where $n$ is the bit width and $p$ is the fixed binary-point position under the paper's two's-complement fixed-point convention and sign assumptions.

**Qubit count:** $(d+1)n + \lceil \log_2 M \rceil + 1$.

### Automatic compilation via Remez algorithm

The software module takes a function $f$, target precision $\varepsilon$, polynomial degree $d$, and domain $\Omega = [a, a+L)$ as inputs. It runs the Remez algorithm to find the minimax polynomial of degree $d$ on progressively smaller subintervals until the $L_\infty$ error falls below $\varepsilon$. This determines the partition $\{\Omega_i\}$ and the polynomial coefficients. The output feeds directly into the parallel evaluation circuit.

### Reversible fast inverse square root

Computing $1/\sqrt{a}$ via Newton iteration $x_{n+1} = x_n(1.5 - ax_n^2/2)$:

**Initial guess.** Find the position of the leading 1-bit in $a$ (costing $2n$ Toffolis for the flag scan), then initialize:

$$x_0 = 2^k \left(C(k) - \frac{a \cdot 2^{3k}}{2}\right)$$

where $k = \lfloor(p - \lfloor \log_2 a \rfloor)/2 \rfloor$ and $C(k)$ is a tuned constant (1.613, 1.5, or 1.62 depending on the sign of $k$). Tuning $C(k)$ to three cases eliminates the error peaks near powers of two that appear in the raw Quake III approach, saving one full Newton iteration.

**Newton iteration circuit.** Each iteration performs: square $x_n$, multiply by $a$, subtract from 1.5, multiply by $x_n$, then uncompute intermediates. Cost per iteration:

$$T_{\text{iter}}(n, p) = 5T_{\text{mul}}(n, p) + 2T_{\text{add}}(n) = \tfrac{15}{2}n^2 + 15np + \tfrac{23}{2}n - 15p^2 + 15p - 2$$

Total for $m$ iterations:

$$T_{\text{invsqrt}}(n, m, p) = n^2\!\left(\tfrac{15}{2}m + 3\right) + 15npm + n\!\left(\tfrac{23}{2}m + 5\right) - 15p^2 m + 15pm - 2m$$

Qubits: $n(m+4)$. Typical configurations: $m = 3$ iterations with $n = 35$ bits gives $\sim 10^{-10}$ absolute error.

### Arcsine via Cephes identity

For $x \in [0, 0.5)$: approximate $\arcsin(x)$ directly with a minimax polynomial.

For $x \in [0.5, 1]$: use the double-angle identity

$$\arcsin(x) = \frac{\pi}{2} - 2\arcsin\!\left(\sqrt{\frac{1-x}{2}}\right)$$

which maps the argument back into $[0, 0.5]$ where the polynomial works. Computing $\sqrt{z}$ uses the inverse square root followed by $x \cdot (1/\sqrt{x})$.

Total Toffoli count for arcsin on $n$-bit numbers with $m$ Newton iterations and degree-$d$ polynomial:

$$T_{\text{arcsin}} = d(3n^2 + n(6p+7) - 6(p-1)p - 2) + m(n(15n + 30p + 23) - 30p(p-1) - 4) + \tfrac{9}{2}n(n+1) + 9(n+1)p + 6n^2 + 28n - 9p^2 + 2$$

### Fixed-point arithmetic primitives

All circuits use $n$-bit fixed-point two's-complement with a fixed binary point position $p$. Key primitives:

- **Multiplication:** Repeated addition-and-shift with pre-truncation, costing $T_{\text{mul}}(n, p) = \frac{3}{2}n^2 + 3np + \frac{3}{2}n - 3p^2 + 3p$ Toffolis. The multiplicand is assumed positive; negative multipliers are handled by a conditional subtraction on the MSB.

- **Squaring:** Same cost as multiplication but saves $n - 1$ qubits by copying out one bit at a time instead of duplicating the full register.

- **Addition:** Uses the Takahashi et al. (2009) circuit: $3n + 3$ Toffolis for controlled addition of two $n$-bit numbers.

## Key results

**Piecewise polynomial evaluation (selected from Table II):** these are compute-only counts. Full reversible use normally doubles them for uncomputation.

| Function | $L_\infty$ error | Degree | Subintervals | Qubits | Toffolis (compute only) |
|---|---|---|---|---|---|
| $\tanh(x)$ | $10^{-7}$ | 4 | 23 | 205 | 23,095 |
| $e^{-x^2}$ | $10^{-7}$ | 4 | 15 | 199 | 19,090 |
| $\sin(x)$ | $10^{-7}$ | 4 | 2 | 176 | 11,480 |
| $e^{-x}$ | $10^{-7}$ | 4 | 15 | 184 | 15,690 |
| $\arcsin(x)$ on $[-0.5, 0.5]$ | $10^{-7}$ | 4 | 2 | 166 | 9,419 |

Uncomputation doubles the Toffoli count for any of these.

**Inverse square root:**

$$T_{\text{invsqrt}} = O(mn^2) \text{ Toffolis}, \quad n(m+4) \text{ qubits}$$

With tuned initial guess, 3 Newton iterations at $n = 35$ bits achieve $\sim 10^{-10}$ absolute error. Without tuning, 4 iterations are needed for the same precision.

**Arcsine on $[0, 1]$:**

The full arcsine (with square root subroutine) costs roughly twice the inverse square root plus one polynomial evaluation. For $n = 35$ bits, $m = 3$ Newton iterations, degree 7 polynomial: $\sim 10^{-7}$ error.

## Comparison with prior work

| Approach | Functions | Primitives used | Key advantage | Key disadvantage |
|---|---|---|---|---|
| **Häner-Roetteler-Svore (this paper)** | General smooth functions, $1/\sqrt{x}$, $\arcsin$ | Piecewise polynomial + Newton | Low degree polynomials, few registers, automatic compilation | High Toffoli count from multiplication overhead |
| Cao-Papageorgiou-Petras-Traub-Kais (2013) | $\sin(x)$ and inverse | Multiple $\sin$ evaluations | Existing construction | Costs proportional to multiple $\sin$ invocations |
| Bhaskar-Hadfield-Papageorgiou-Petras (2016) | $1/\sqrt{x}$ | Newton iteration | Similar idea | Worse initial guess (one extra Newton iteration) |
| [[Quantum CORDIC — Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024) — Paper Notes|Burge-Barbeau-Garcia-Alfaro (2024)]] | $\arcsin$ | CORDIC rotations | $O(n)$ qubits, no multiplications | $O(n^2)$ CNOTs, $O(n \log n)$ depth; later work |
| [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] | Avoids arcsine entirely | Inequality test | 286--375x fewer Toffolis than Grover+arcsine | Only for state preparation, not general function evaluation |
| [[Adaptive QROM for Function Evaluation|QROM + interpolation (Sanders et al. 2020)]] | Smooth functions | Piecewise linear via QROM | One multiplication only | Approximate; precision limited by table size |

**Assessment:** This paper fills a practical gap: before 2018, there were no published gate-level implementations of functions like $\tanh$, Gaussian, or $\arcsin$ with concrete Toffoli counts. The piecewise polynomial approach is general and automatic, which was new. The Newton-based inverse square root adapts a well-known classical trick effectively. The circuits are expensive in absolute terms (tens of thousands of Toffolis for moderate precision), which later motivated approaches that avoid arithmetic entirely (Sanders-Low-Scherer-Berry) or use QROM table lookup with refinement. Still, for applications where exact function evaluation on a quantum register is unavoidable, these circuits remain a useful reference point.

## Limits / caveats

1. **Multiplication dominates cost.** Each Horner iteration and each Newton step involves at least one $n$-bit multiplication at $O(n^2)$ Toffolis. For high precision ($n > 50$), this becomes very expensive. Karatsuba multiplication ([[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes|Parent-Roetteler-Mosca 2017]]) could reduce this to $O(n^{1.585})$ but is not used here.

2. **Fixed-point only.** The circuits use fixed-point arithmetic, not floating-point. This means the binary point position must be chosen to avoid overflow for the worst-case input, wasting precision on inputs far from the boundary. [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes|Wiebe-Kliuchnikov (2013)]] discusses floating-point representations but the two approaches were not combined.

3. **No T-count optimisation.** The paper counts Toffoli gates but does not apply [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney's temporary logical-AND]] (2018, same year) to reduce T-count of paired Toffolis. Applying it post hoc would roughly halve the T-cost.

4. **Uncomputation doubles cost.** All Toffoli counts in Table II are compute-only; the full reversible circuit is $2\times$. The paper does not explore measurement-based uncomputation.

5. **Arithmetic vs. QROM interpolation.** Arithmetic evaluation is natural when the function must be evaluated at high precision or on a large dynamic domain. QROM/table interpolation is often cheaper when the domain can be discretized classically and table lookup cost is acceptable, because it replaces repeated multiplications by data loading plus low-degree refinement.

6. **Superseded for specific use cases.** For state preparation (the main application of $\arcsin$), [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] showed how to avoid the arcsine entirely, reducing Toffoli counts by orders of magnitude. For coarse function evaluation in heuristic algorithms, [[Adaptive QROM for Function Evaluation|QROM-based interpolation]] is cheaper. These circuits remain relevant mainly when exact evaluation of a specific function at moderate-to-high precision is required inside a quantum oracle.

## Reusable ideas

1. [[Parallel Piecewise Polynomial Evaluation]] — Evaluate $M$ different low-degree polynomials on $M$ subintervals simultaneously using a label register and controlled coefficient loads, turning domain partitioning from an $O(M)$-cost sequential loop into an $O(\log M)$-overhead parallel scheme.

2. [[Reversible Newton-Raphson with Tuned Initial Guess]] — Adapt classical Newton-Raphson iteration for quantum circuits by storing iterates in separate registers and using [[Reversible Computation via Compute-Copy-Uncompute|compute-copy-uncompute]] for cleanup. The initial-guess tuning (three magic constants depending on the exponent) saves one full iteration, reducing cost by 20--25%.

3. [[Remez-Based Automatic Oracle Compilation]] — Automatically partition a domain and fit minimax polynomials via the Remez algorithm, then compile the result into a parallel evaluation circuit. This turns the problem of quantum function evaluation from manual circuit design into a classical optimisation problem.

## References within this paper

- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner-Roetteler-Svore (2017)]] — the authors' own prior work; the CARRY circuit and in-place constant adder are reused here.
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] — QFT-based adder, cited as a comparator approach.
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro-Draper-Kutin-Moulton (2004)]] — alternative adder cited for comparison.
- Takahashi, Tani, Kunihiro (2009) — the controlled addition circuit ($3n + 3$ Toffolis) used as the base adder in this paper's multiplier.
- Parent, Roetteler, Svore (2015) — reversible circuit compilation with space constraints; the pebbling strategies in Table I.
- Bennett (1989) — time/space tradeoffs for reversible computation; the pebble game model.
- Remez (1934) — the minimax polynomial approximation algorithm used in the software stack module.
- Bhaskar, Hadfield, Papageorgiou, Petras (2016) — prior work on quantum inverse square root; this paper improves the initial guess.
- Lomont (2003) — review of the fast inverse square root from Quake III Arena.
- SL Moshier, Cephes math library (2000) — the classical implementation of arcsine that this paper adapts.
- Barenco et al. (1995) — multi-controlled NOT construction from dirty ancillae.
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher et al. (2017)]] — FeMoCo benchmark motivating the need for quantum arithmetic.
- Babbush, Berry, Kivlichan, Wei, Love, Aspuru-Guzik (2016) — quantum chemistry simulation requiring Gaussian and $1/\sqrt{x}$ evaluation.

## Cross-links

### Paper notes
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — same authors' earlier work; circuits for CARRY, constant adder, and modular multiplication are reused
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — published the same year; the [[Temporary Logical-AND]] technique could halve the T-count of this paper's Toffoli-heavy circuits
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — Karatsuba multiplication and [[Pebble Game Space Reduction for Recursive Circuits|pebble games]] could reduce the $O(n^2)$ multiplication cost
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]] — later work that eliminates the need for arcsine in state preparation, citing this paper's circuits as the cost baseline
- [[Quantum CORDIC — Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024) — Paper Notes]] — later alternative approach to arcsine using CORDIC rotations instead of polynomial + Newton
- [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes]] — later work by Häner and Roetteler on chemistry resource estimation
- [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes]] — floating-point rotation synthesis; a complementary approach to fixed-point arithmetic
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — Svore's FeMoCo benchmark motivating these circuits

### Trick cards
- [[Parallel Piecewise Polynomial Evaluation]] — the main general-purpose technique from this paper
- [[Reversible Newton-Raphson with Tuned Initial Guess]] — the Newton-based inverse square root technique
- [[Remez-Based Automatic Oracle Compilation]] — the automatic partitioning and circuit generation module
- [[Pebble Game Space Reduction for Recursive Circuits]] — used for space/time tradeoff in polynomial evaluation
- [[Reversible Computation via Compute-Copy-Uncompute]] — the standard pattern used throughout for uncomputation
- [[Dirty Ancilla Constant Addition]] — the constant adder from the authors' 2017 paper, reused here
- [[Adaptive QROM for Function Evaluation]] — a later alternative approach to function evaluation
- [[QROM + Newton Iteration for Function Evaluation]] — a later hybrid QROM/Newton approach
