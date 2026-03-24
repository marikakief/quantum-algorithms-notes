# Repeat-Until-Success with Clifford Correction

> **Source:** Wiebe, Kliuchnikov, arXiv:1305.5528 (also Paetznick-Svore 2013, arXiv:1311.1074)
> **Tags:** #trick #gate-synthesis #non-deterministic #fault-tolerance #repeat-until-success

## What it does

Turns a non-deterministic quantum circuit whose failure mode is a known Clifford into a correct gate synthesis protocol: on failure, apply the inverse Clifford correction and retry. Expected T-count is $T_{\text{attempt}} / P_{\text{success}}$; no wasted state preparation.

## The trick

Design the circuit so that:
- **Success** (probability $P$): the target unitary $U$ is applied.
- **Failure** (probability $1-P$): a known Clifford $C$ is applied (possibly depending on which measurement outcome occurred, but always a Clifford — i.e., cheap).

On failure: apply $C^\dagger$ (cost: zero T gates), then retry from the original state.

Because Clifford gates are cheap (no T gates in fault-tolerant settings), the correction adds no T overhead. Expected T-count per success = $T_{\text{attempt}} / P$.

For the [[Gearbox Circuit for Angle Squaring]], the failure Clifford is $e^{i\pi X/4}$, so its inverse is trivial. For general RUS circuits, the failure Cliffords may differ by measurement outcome but all have T-count 0.

**Key design criterion:** to get a valid RUS protocol, you need the failure branches to all produce Cliffords, not arbitrary unitaries. This places a constraint on how the ancilla is coupled to the target.

## When to reach for it

- Whenever a probabilistic quantum gate construction has failure modes that are (or can be made) Clifford operations.
- Particularly useful for single-qubit gate synthesis where ancilla-assisted circuits give better asymptotic T-count than ancilla-free methods.
- When $P$ is close to 1 (e.g., $P \approx 1 - O(\theta^4)$ for small $\theta$), the expected overhead is negligible.

## Complexity

Expected T gates per successful implementation:
$$\mathbb{E}[T] = \frac{T_{\text{attempt}}}{P_{\text{success}}}$$

Expected number of attempts: geometric with mean $1/P$.

For $P = 1 - \delta$: $\mathbb{E}[\text{attempts}] = 1/(1-\delta) \approx 1 + \delta$ for small $\delta$. Variance in T-count is $\text{Var}[T] = T_{\text{attempt}}^2 (1-P)/P^2$.

## Caveat

- If $P$ is not close to 1, expected T-count can be large. Verify the success probability before committing to an RUS architecture.
- Mid-circuit measurement is required — if your hardware or error model makes measurements expensive, RUS circuits may not be net beneficial.
- Must be able to reinitialise or reset the ancilla after failure.
- Classically controlled Clifford corrections require classical processing and control-flow between rounds.

## Related notes

- [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes]]
- [[Gearbox Circuit for Angle Squaring]]
- [[Floating-Point Angle Decomposition for Rotation Synthesis]]
