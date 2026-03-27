
> **Source:** Costa, An, Babbush, Berry, arXiv:2312.07690
> **Tags:** #trick #resource-estimation #constant-factors #numerical-benchmarks #QLSP

## What it does

Tests the actual constant factor in a proven asymptotic bound by running circuit-level numerical simulations on random problem instances, rather than relying solely on the (often very loose) analytical upper bound.

## The trick

When you prove $T = O(f(\text{params}))$, the proof usually goes through multiple triangle inequalities, operator norm bounds, and worst-case estimates. The resulting constant can be orders of magnitude too large.

To get the real constant: implement the actual quantum circuit (or a classical simulation of it), generate random instances covering the parameter range of interest, and measure the empirical relationship between $T$ and the parameters. Fit $T = \alpha \cdot f(\text{params})$ to extract $\alpha$.

In the QLSP case: the analytical bound gives $T = 2305\kappa/\Delta$ (PD Hermitian) for the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic walk]]. Numerical testing across 100 random matrices per condition number, for $\kappa \in \{10, 20, 30, 40, 50\}$ and dimensions $4\times4$ through $16\times16$, reveals $\alpha \approx 1.56$ — about 1,500× tighter.

The key methodological choice: simulate the full [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk circuit, not just the abstract Hamiltonian evolution. This catches implementation-level effects that the analytical bound may over- or under-estimate.

## When to reach for it

- After proving an asymptotic result with explicit constants, before using those constants for resource estimation.
- When comparing two algorithms that have different constant factors but the same or similar asymptotic scaling.
- When an algorithm looks impractical based on its analytical bound but the proof technique involves many layers of relaxation.
- Before committing to resource estimates for fault-tolerant implementations — the difference between $\alpha = 2305$ and $\alpha = 1.56$ is the difference between "infeasible" and "promising."

## Complexity

Depends on the simulation. For the QLSP case: classical simulation of the walk on $N \times N$ matrices costs $O(N^2)$ per step and $O(T)$ steps. Feasible for $N \leq 16$ with hundreds of instances; larger sizes may need sparse matrix techniques or sampling.

## Caveat

Random matrices may not represent the structure of real problem instances. The constant could be better (exploitable structure) or worse (adversarial instances) for specific applications. The numerical constant is also not a proven bound — it's an empirical observation. You know the algorithm works because of the analytical proof, but you don't have a tight *proven* constant.

Also: if the constant turns out to be large for a reason (e.g., worst-case spectral structure that random matrices don't exhibit), the numerical test will miss it.

## Related notes
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[Discrete Adiabatic Theorem for Quantum Walks]]
