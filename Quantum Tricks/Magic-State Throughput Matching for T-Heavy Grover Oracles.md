# Magic-State Throughput Matching for T-Heavy Grover Oracles

> **Source:** Amy, Di Matteo, Gheorghiu, Mosca, Parent, Schanck, arXiv:1603.09383
> **Tags:** #trick #magic-state-distillation #surface-code #resource-estimation #Grover

## What it does

Sizes the number of magic-state distilleries by the circuit's T-width so T-state supply, not factory under-provisioning, sets the Grover runtime.

## The trick

For a logical circuit $U$, compute
$$
T^w_U=\frac{T^c_U}{T^d_U},
$$
where $T^c_U$ is T-count and $T^d_U$ is T-depth. If one distillery produces $\phi$ T states every $\sigma_{\mathrm{dist}}$ surface-code cycles, then maintaining one T-depth layer every $\sigma_{\mathrm{dist}}$ cycles needs roughly
$$
\Phi = \left\lceil \frac{T^w_U}{\phi}\right\rceil
$$
parallel distilleries.

In the SHA pre-image estimates, a 15-to-1 Reed-Muller stack produces about four magic states every $530$ surface-code cycles. SHA-256 has average T-width about $3$, so one distillery keeps pace. SHA3-256 has T-width about $1175$, so the paper allocates about $294$ distilleries.

## When to reach for it

- Surface-code resource estimates where T gates dominate runtime.
- Comparing two oracle circuits with similar T-count but very different T-depth.
- Deciding whether to spend qubits on more factories or accept longer runtime.
- Grover-style attacks, where the same T-heavy oracle is repeated $\Theta(\sqrt N)$ times.

## Complexity

The area cost grows linearly in $\Phi$:
$$
N_{\mathrm{total}}=N_{\mathrm{data}}+\Phi N_{\mathrm{dist}}.
$$
The time per T-depth layer is about $\sigma_{\mathrm{dist}}$ once enough factories are provisioned. Under-provisioning increases runtime by the T-state supply deficit.

## Caveat

T-width is an average. Real schedules have bursts, routing constraints, Pauli-frame updates, and measurement dependencies. A serious layout can need buffering or extra factories beyond the average-width estimate.

## Related notes

- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
- [[Logical-Qubit-Cycle Costing for Grover Attacks]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
