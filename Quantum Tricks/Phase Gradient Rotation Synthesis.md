# Phase Gradient Rotation Synthesis

> **Source:** Gidney, Quantum **2**, 74 (2018); applied in Low, Kliuchnikov, Schaeffer, arXiv:1812.00954
> **Tags:** #trick #circuit-primitive #T-complexity #rotation-synthesis #phase-gradient #fault-tolerant

## What it does

Implements arbitrary controlled-$Z$ rotations at $O(b)$ T-gate cost (where $b$ is the precision in bits), instead of the $O(\log(1/\varepsilon))$ T-gate cost of standard single-qubit synthesis (e.g., Ross-Selinger). The trick: prepare a Fourier resource state once, then reuse it for all rotations via reversible addition.

## The trick

Prepare the **phase gradient state** (Fourier state):

$$|F\rangle = \frac{1}{\sqrt{2^b}} \sum_{k=0}^{2^b - 1} e^{-2\pi i k / 2^b} |k\rangle$$

This costs $O(b \log(1/\varepsilon))$ T gates to prepare to error $\varepsilon$, but is a **one-time cost** shared across all subsequent rotations.

Given a $b$-bit approximation $a_x$ of the desired rotation angle $\theta_x$ (loaded into a register by a data-lookup oracle like [[SelectSwap Network for Data Lookup|SelectSwap]] or [[QROM (Quantum Read-Only Memory)|QROM]]), apply a **controlled reversible adder**:

$$\text{CAdd}|a_x\rangle|F\rangle|z\rangle = |a_x\rangle|F\rangle\, e^{2\pi i a_x Z / 2^b}|z\rangle$$

This works because $\text{Add}|a_x\rangle|F\rangle = e^{2\pi i a_x / 2^b}|a_x\rangle|F\rangle$ — the Fourier state is an eigenstate of addition with eigenvalue $e^{2\pi i a_x/2^b}$. The controlled adder costs $O(b)$ Clifford+T gates.

The total error per rotation is $O(2^{-b})$ from angle truncation. With $b = O(\log(\log N/\varepsilon))$ bits, this is sufficient for state preparation of an $N$-dimensional state to error $\varepsilon$.

## When to reach for it

- Any time you need many controlled rotations with different angles (state preparation, multiplexed rotations, PREPARE circuits).
- Especially valuable when the rotation angles are already available in a quantum register from a data-lookup step.
- Replaces the standard approach of synthesising each rotation independently at $O(\log(1/\varepsilon))$ T gates per rotation.
- Used inside the state preparation algorithm of [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes|Low-Kliuchnikov-Schaeffer (2024)]] to avoid the per-rotation synthesis overhead.

## Complexity

| Component | T-cost |
|-----------|--------|
| Fourier state preparation | $O(b\log(1/\varepsilon))$ (one-time) |
| Each controlled rotation | $O(b)$ |
| Total for $M$ rotations | $O(b\log(1/\varepsilon) + Mb)$ |

Compare: standard synthesis of $M$ rotations costs $O(M\log(1/\varepsilon))$. The phase gradient approach wins when $M \gg 1$.

## Caveat

- The Fourier state must be prepared to high precision — its error contributes directly to the output state error. But this is a one-time cost.
- The $O(b)$ cost per rotation is in Clifford+T gates (the adder uses Toffoli gates). The *T-only* cost per rotation is $O(b)$ Toffolis, each decomposing into 4–7 T gates.
- If you only need one or two rotations, standard synthesis is simpler.
- The technique assumes the rotation angles are available in a quantum register. If angles must be computed on the fly (not loaded from a table), the data-lookup cost dominates.

## Related notes
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes]]
- [[Phase Gradient State for Controlled Rotations]] — the underlying resource state trick
- [[SelectSwap Network for Data Lookup]] — loads the angle data into registers
- [[QROM (Quantum Read-Only Memory)]] — alternative data-loading approach
- [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes]] — related rotation synthesis techniques
