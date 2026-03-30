# Offline Resource State Preparation for Gate Teleportation

> **Source:** E. Knill, R. Laflamme, G. J. Milburn, arXiv:quant-ph/0006088; see also D. Gottesman, I. Chuang, arXiv:quant-ph/9908010
> **Tags:** #trick #gate-teleportation #fault-tolerance #state-preparation #KLM

## What it does

Converts non-deterministic (probabilistic) gates into near-deterministic ones by moving the non-determinism into offline resource state preparation, where failure can be tolerated and retried without disturbing the main computation.

## The trick

Suppose you have a gate $G$ that works with probability $p < 1$, but failure is detectable. You want to apply $G$ reliably in a running computation.

The [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes|Gottesman-Chuang]] gate teleportation protocol says: instead of applying $G$ directly, prepare an entangled resource state $|\chi_G\rangle$ that has $G$ "baked in." Then teleport your data qubits through $|\chi_G\rangle$. The output is $G|\psi\rangle$ up to a known Pauli correction (which is cheap to fix if $G$ normalises the Pauli group, or manageable via the Clifford hierarchy otherwise).

The resource state $|\chi_G\rangle$ can be prepared offline:
1. Attempt to prepare $|\chi_G\rangle$ using the non-deterministic gate
2. If preparation fails (detected), discard and retry
3. Expected number of trials: $1/p$
4. Once you have $|\chi_G\rangle$, store it and use it on demand

This separates the **time** of state preparation (which can be slow) from the **moment** of gate application (which needs to succeed on schedule). The main computation only proceeds when a verified resource state is available.

In KLM's protocol, the teleportation through an $n$-mode resource state $|t'_n\rangle$ succeeds with probability $(n/(n+1))^2$, and failure is a detected erasure. For $n = 25$, the per-gate failure rate is $\sim 8\%$, well within the [[Detected-Error Threshold Boosting|detected-error threshold]].

## When to reach for it

- When you have a useful but unreliable quantum operation (probabilistic gate, non-deterministic state conversion, noisy preparation)
- When the operation's failure is detectable (heralded)
- When you can afford preparation time but need deterministic execution in the main circuit
- Magic state distillation for fault-tolerant $T$ gates is a direct descendant of this idea — prepare noisy magic states offline, distil them to high fidelity, then inject via gate teleportation

## Complexity

Preparation cost: $O(1/p)$ expected trials per resource state, where $p$ is the non-deterministic gate's success probability. For KLM with recursive construction: $2^{O(\sqrt{n})}$ total operations per resource state.

Online cost: One teleportation step per gate application — $O(n)$ beam splitters and $O(n)$ photon detections plus classical feed-forward.

## Caveat

The resource states must be stored coherently until needed. In photonic systems, this means optical delay lines or quantum memories — both lossy. Storage loss directly reduces the effective success probability and can dominate the error budget.

Also, the Pauli correction after teleportation must be applied *before* the next gate. This classical feed-forward latency sets a minimum gate cycle time, which limits the clock speed of the computer.

## Related notes
- [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes]]
- [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes]]
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]]
- [[Measurement-Induced Nonlinearity]]
- [[Detected-Error Threshold Boosting]]
