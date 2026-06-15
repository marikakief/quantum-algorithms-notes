# Retry-Aware Surface-Code Resource Accounting

> **Source:** Gidney and Ekerå, arXiv:1905.09749
> **Tags:** #trick #resource-estimation #fault-tolerance #surface-code #cryptanalysis

## What it does

Estimates fault-tolerant algorithm cost by optimising expected spacetime volume after all major failure channels, rather than quoting a single clean logical circuit size.

## The trick

For a proposed fault-tolerant implementation, estimate per-run space $s$, per-run time $t$, and total retry risk $\epsilon$. Then optimise a cost such as

$$
\frac{s t}{1-\epsilon}
$$

or a skewed version such as

$$
s^{1.2}\frac{t}{1-\epsilon},
$$

when one wants to prefer saving physical qubits over saving time. The retry risk is not just logical gate failure. In Gidney--Ekerå it includes:

1. **Topological error:** logical failures during surface-code storage and operations.
2. **Distillation error:** bad CCZ/T states that pass the factory checks.
3. **Approximation error:** trace-distance error from [[Coset Representation for Approximate Modular Arithmetic]] and [[Oblivious Carry Runway|oblivious carry runways]].
4. **Classical post-processing failure:** e.g. the $<1\%$ failure probability in Ekerå's short-log post-processing after a correct quantum run.

The key move is to scan implementation parameters jointly: code distances, factory type, window sizes, padding, runway spacing, and layout parallelism. A lower logical error rate is not automatically better if it requires enough extra code distance to lose on expected volume.

## When to reach for it

- Cryptanalytic resource estimates where the end user cares about wall-clock time and physical qubits, not just Toffoli count.
- Surface-code designs where increasing code distance trades space for retry probability.
- Approximate arithmetic or approximate simulation algorithms where algorithmic error and hardware error must share one budget.

## Complexity

The accounting cost is dominated by the optimiser: evaluate all candidate parameter tuples and compute $s,t,\epsilon$ for each. In Gidney--Ekerå's RSA estimates, the scan ranges over small sets of code distances, $c_{\exp},c_{\mathrm{mul}}\in\{4,5,6\}$, $c_{\mathrm{sep}}\in\{512,768,1024,1536,2048\}$, and several padding offsets.

## Caveat

The output is only as good as the physical model. Cycle time, reaction time, factory layout, physical gate error, routing assumptions, and decoder performance can each move constants substantially. This trick improves honesty; it does not remove modelling uncertainty.

## Related notes

- [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]]
- [[Logical-Qubit-Cycle Costing for Grover Attacks]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
- [[Coset Representation for Approximate Modular Arithmetic]]
- [[Oblivious Carry Runway]]
