# Remez-Based Automatic Oracle Compilation

> **Source:** Häner, Roetteler, Svore, arXiv:1805.12445
> **Tags:** #trick #quantum-arithmetic #oracle-compilation #function-evaluation #piecewise-polynomial #Remez-algorithm

## What it does
Automatically compiles a classical smooth function $f(x)$ over a domain $\Omega$ into a quantum circuit, given a target precision $\varepsilon$ and a polynomial degree budget $d$. The output is a [[Parallel Piecewise Polynomial Evaluation|parallel piecewise polynomial evaluation]] circuit.

## The trick
The problem of designing a quantum circuit for $f$ is reduced to a classical optimisation problem:

1. **Binary search on interval width.** Start with the full domain $\Omega_1 = [a, a+L)$. Run the Remez algorithm to find the degree-$d$ minimax polynomial $P(x)$ for $f$ on $\Omega_1$. If the $L_\infty$ error exceeds $\varepsilon$, halve the domain width and repeat via binary search until finding the largest subinterval $\Omega_1 = [a, b_1)$ where degree $d$ suffices.

2. **Iterate.** Set $\Omega_2 = [b_1, b_2)$ using the same procedure, starting from $b_1$. Continue until the entire domain is covered: $b_k \geq a + L$.

3. **Compile.** The set of subintervals $\{\Omega_i\}$ and their polynomial coefficients feed directly into the parallel evaluation circuit. The number of subintervals $M$ is determined automatically.

The Remez algorithm finds the unique polynomial of degree $d$ minimizing the worst-case ($L_\infty$) error on a given interval — this is the best possible approximation in the minimax sense. Functions with large higher derivatives need more subintervals; very smooth functions (like $\sin$) need few.

## When to reach for it
- Compiling any smooth classical function into a quantum oracle without manual circuit design.
- When the function is provided as a black box (callable in classical code) and you need a fault-tolerant circuit.
- Integrating into a quantum software stack (Q#, ProjectQ, Quipper) for automatic oracle generation.

## Complexity
Determined by the output: the number $M$ of subintervals depends on $f$, $\varepsilon$, and $d$. The circuit cost is $T_{\text{pp}}(n, d, p, M)$ as given in [[Parallel Piecewise Polynomial Evaluation]]. The classical compilation cost (running Remez) is negligible compared to the quantum runtime.

## Caveat
Only works for piecewise smooth functions. Functions with singularities ($1/\sqrt{x}$ near 0, $\arcsin$ near $\pm 1$) need separate treatment — the paper handles these via Newton iteration, not polynomial approximation. The degree $d$ is fixed across all subintervals, which may not be optimal (adaptive degree per interval could reduce $M$). The Remez algorithm requires evaluating $f$ at many classical points, which is fine for standard functions but may be a bottleneck for expensive-to-evaluate functions.

## Related notes
- [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]]
- [[Parallel Piecewise Polynomial Evaluation]]
- [[Adaptive QROM for Function Evaluation]]
- [[Pebble Game Space Reduction for Recursive Circuits]]
