# Logical-Qubit-Cycle Costing for Grover Attacks

> **Source:** Amy, Di Matteo, Gheorghiu, Mosca, Parent, Schanck, arXiv:1603.09383
> **Tags:** #trick #Grover #resource-estimation #surface-code #quantum-cryptanalysis

## What it does

Turns a black-box Grover query estimate into an area-time cost by charging for every logical qubit during every surface-code cycle.

## The trick

For a fault-tolerant Grover attack, do not stop at the query count
$$
\left\lfloor \frac{\pi}{4}\sqrt{N}\right\rfloor.
$$
Build the reversible predicate, compile it to a fault-tolerant gate set, then estimate:

- $N_{\mathrm{data}}$: logical qubits in the oracle and search registers;
- $N_{\mathrm{dist}}$: logical qubits used by each magic-state distillery;
- $\Phi$: number of parallel distilleries;
- $\sigma$: number of surface-code cycles for all Grover iterations.

The cost is
$$
C_{\mathrm{LQC}}=(N_{\mathrm{data}}+\Phi N_{\mathrm{dist}})\sigma.
$$

If the Grover computation has polynomial overhead $k^v$ on a $k$-bit search space, the finite-size budget equation is
$$
\frac{k}{2}+v\log_2 k\le C,
$$
for total cost $2^C$. Equality can be solved as
$$
k(v,C)=\frac{2v}{\ln 2}\, W\!\left(\frac{2^{C/v}\ln 2}{2v}\right).
$$

## When to reach for it

- Comparing a quantum brute-force attack to classical exhaustive search.
- Auditing claims of “$k$ bits become $k/2$ bits” security under [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]].
- Estimating symmetric-key or hash-function security when the reversible oracle is nontrivial.
- Separating query complexity from implementation cost in a fault-tolerant setting.

## Complexity

You need a logical circuit estimate, a T-count/T-depth estimate, and a surface-code schedule. The final cost is an area-time product, not just a gate count.

## Caveat

This is a cost model, not a theorem. The result depends on code choice, cycle time, distillation scheme, physical error rate, and the convention used to compare a surface-code cycle with a classical operation.

## Related notes

- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Magic-State Throughput Matching for T-Heavy Grover Oracles]]
