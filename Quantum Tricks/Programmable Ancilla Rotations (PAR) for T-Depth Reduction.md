# Programmable Ancilla Rotations (PAR) for T-Depth Reduction

> **Source:** Jones et al., New J. Phys. **14**, 115023 (2012); resource analysis in Reiher, Wiebe, Svore, Wecker, Troyer, arXiv:1605.03590
> **Tags:** #trick #T-depth #rotation-synthesis #fault-tolerant #magic-state-distillation #parallelism

## What it does
Reduces the T-depth of rotation-heavy circuits by pre-computing rotations as resource states in dedicated factories and teleporting them into the computation, trading T-depth for ancilla qubits and T factories.

## The trick
To perform $R_z(\theta)$:

1. **Pre-cache** the states $R_z(2^j\theta)|+\rangle$ for $j = 0, 1, \ldots, n-1$ in dedicated rotation factories.
2. **Attempt** to teleport $R_z(\theta)$ using the $j=0$ state with a Clifford circuit. Succeeds with probability 1/2.
3. **On failure**, the circuit applies $R_z(-\theta)$ instead. Correct by teleporting $R_z(2\theta)$ from the $j=1$ cache.
4. **Repeat** up to $n$ levels. If all $n$ attempts fail (probability $2^{-n}$), fall back to online rotation synthesis at cost $C$ T gates.

For $M$ rotations cached with $n$ correction levels:
- Expected number of rotations before a failure: $2^n(1 - (1-2^{-n})^M)$
- Expected online cost per rotation: $(2 - n + 2/2^n) + (C+n)/2^n$ T gates (Theorem 3 in Reiher et al.)
- With factory-based just-in-time production: $nC$ factories continuously produce correction states, staggered so one set finishes before the next rotation is needed.

## When to reach for it
- [[Product Formula (Trotter-Suzuki)|Trotter-Suzuki]] simulation circuits with many sequential rotations (the original use case)
- Any algorithm where the rotation angles are known in advance (they can be pre-cached)
- When T-depth (wall-clock time) matters more than total T count

Largely superseded for chemistry by [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]], which replaces rotations with Toffoli-based [[QROM (Quantum Read-Only Memory)|QROM]] lookups. But PAR remains relevant for any fault-tolerant algorithm that still uses explicit rotations.

## Complexity
For the Reiher et al. FeMoCo simulation: PAR reduces runtime from 130 days (serial) to 110 hours, but increases logical qubits from 111 to 1,982 and requires ~166,000 T factories at $10^{-3}$ error rate.

General scaling: T-depth reduced by factor $\sim 2^n$; space increased by factor $\sim nC$ (number of parallel factories).

## Caveat
The qubit overhead is severe. At low error rates ($10^{-3}$) the number of T factories — each requiring $\sim 10^6$ physical qubits — dominates the total physical qubit count, pushing into the $10^{11}$ range for FeMoCo PAR. The [[Modular Quantum Computer Architecture for Chemistry|modular architecture]] helps (factories can be mass-produced special-purpose devices), but it's still daunting.

For modern qubitization-based chemistry, PAR is irrelevant — the inner loop uses Toffolis and QROM, not rotations. The trick is most useful where Trotter simulation or rotation-heavy circuits are still the algorithm of choice.

## Related notes
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]]
- [[Sign-Conditioned Phase Estimation (Ito-Wiebe Trick)]]
- [[Modular Quantum Computer Architecture for Chemistry]]
- [[Error Budget Optimization for Fault-Tolerant Algorithms]]
