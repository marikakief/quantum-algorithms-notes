# Median TTS Benchmark for Quantum Heuristics

> **Source:** Boulebnane--Montanaro, arXiv:2208.06909
> **Tags:** #trick #benchmarking #QAOA #SAT #time-to-solution

## What it does
Compares a quantum sampler to classical SAT solvers using median time-to-solution exponents rather than raw success probability or expected energy.

## The trick
For a fixed satisfiable instance $\sigma$, QAOA has success probability $p_{\mathrm{succ}}(\sigma)$. Define the instance running time as

$$
T(\sigma)=1/p_{\mathrm{succ}}(\sigma).
$$

For an ensemble of random instances, benchmark the median of $T(\sigma)$ and fit

$$
T_{\mathrm{med}}(n)\approx 2^{\alpha n}.
$$

Compare $\alpha$ against classical solvers measured by formula evaluations or another agreed cost proxy.

## Why it matters
The expected runtime can be infinite if the random ensemble includes unsatisfiable instances. The average success probability can also be too optimistic if easy instances dominate. Median TTS is closer to how SAT solvers are usually benchmarked.

## When to reach for it
Use for random CSPs near thresholds, heuristic quantum samplers, and any setting where the output is a satisfying assignment found by repeated trials.

## Caveat
Cost proxies matter. A QAOA sample is not directly comparable to one Boolean formula evaluation on real hardware. This benchmark is about scaling exponents under an idealised cost model.

## Related notes
- [[Solving Boolean Satisfiability Problems with QAOA (Boulebnane-Montanaro 2022) — Paper Notes]]
- [[Exact-Solution Objective for QAOA]]
