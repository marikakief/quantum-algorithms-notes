# Postselected Computation for State Preparation

> **Source:** Knill, arXiv:quant-ph/0410199
> **Tags:** #trick #fault-tolerance #postselection #state-preparation #threshold

## What it does
Uses postselected quantum computation (abort on detected error, restart) to prepare high-quality encoded states, which are then consumed by standard (non-postselected) fault-tolerant computation. The postselected threshold is higher than the standard threshold, creating a "bridge" regime where hardware errors are too high for direct fault tolerance but low enough for postselected state preparation.

## The trick
Define **postselected quantum computing**: same as standard QC, except each gate may fail (with the failure detected). If any gate fails, restart from scratch. The output is accepted only when all gates succeed.

Key observations:
1. The postselected threshold $\gamma_{\text{post}}$ can be much higher than the standard threshold $\gamma_{\text{std}}$ because postselection converts any detected error into a restart, and detection is easier than correction.
2. Postselected computing alone has limited power (it can't efficiently solve BQP problems because the success probability may be exponentially small). But it can prepare *encoded states* with high fidelity.
3. These encoded states — particularly logical Bell pairs and magic states like $|\pi/8\rangle$ — are all that's needed to run standard fault-tolerant computation via error-correcting teleportation.

The protocol:
1. Use postselected fault-tolerant computing (with its higher threshold) to prepare states encoded in a high-capacity error-correcting code $C_e$
2. Decode the intermediate concatenation layers to get states encoded only in $C_e$
3. Use these $C_e$-encoded states for standard error-correcting teleportation

For Knill's $C_4/C_6$ architecture: $\gamma_{\text{post}} \approx 0.06$, $\gamma_{\text{std}} \approx 0.01$. The bridge regime ($0.01 < \gamma < 0.05$) is accessible only via postselection.

## When to reach for it
- When gate error rates are above the standard fault-tolerance threshold but below the postselected threshold
- When you need high-quality ancilla states (Bell pairs, magic states) but can tolerate probabilistic preparation with retry
- When the bottleneck is state quality rather than state generation rate

## Complexity
The resource overhead is dominated by the expected number of restarts: roughly $(1 - \gamma)^{-k}$ attempts per success, where $k$ is the number of gates that can fail in the preparation circuit. At $\gamma = 3\%$, this is "extreme" (Knill's word). At $\gamma = 1\%$, it's manageable.

## Caveat
- Only works when the computation can be split into "state preparation" (postselectable) and "state consumption" (standard). This is natural for teleportation-based architectures but not for all approaches.
- The postselected threshold depends on the error model. Correlated errors or coherent errors can reduce it.
- Memory errors during the time spent waiting for postselection outcomes (to confirm success before using the state) add to the effective error rate.

## Related notes
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]]
- [[Error-Detecting Codes with Concatenation for Fault Tolerance]]
- [[Error-Correcting Teleportation]]
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
