# Processed Product Formula Kernel-Processor Decomposition

> **Source:** Blanes, Casas, Murua, SIAM J. Sci. Comput. 27, 1817 (2006); improved by Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry, arXiv:2210.15817 (2025)
> **Tags:** #trick #product-formulas #symplectic-integrators #hamiltonian-simulation

## What it does

Reduces the effective per-step cost of a high-order [[Order-Condition Cancellation in Product Formulas|product formula]] by splitting it into a kernel $\Sigma$ and processor $P$, so $S_k = P \Sigma P^{-1}$.

## The trick

For a long-time simulation split into $r$ steps, $S_k^r = P \Sigma^r P^{-1}$. The processor $P$ and $P^{-1}$ are applied once each (at the start and end), so the cost per step is dominated by the kernel $\Sigma$.

The kernel satisfies fewer order conditions than a complete symmetric product formula. At 6th order, the full formula requires $A_{1,m}=1$, $A_{3,m}=0$, $A_{5,m}=0$, $B_{5,m}=0$; the kernel drops the $B_{5,m}=0$ condition. This pattern generalises: at each order, the kernel conditions are a strict subset of the full formula conditions.

Fewer constraints means either:
- **Shorter kernels** — fewer stages $M$ for the same order, or
- **Lower error** — same length but more free parameters to optimize the constant factor

The processor is determined from the kernel by solving the remaining conditions via the (non-symmetric) BCH expansion. The processor conditions involve even-order terms (unlike the kernel, which only has odd-order terms from the symmetric BCH).

## When to reach for it

- You need a high-order product formula ($\geq 6$th order) and the simulation time is long enough that the per-step cost dominates over the one-time processor overhead.
- The number of time steps $r \gg 1$. If $r$ is small, the processor overhead negates the savings.
- Combined with [[Over-Parameterized Product Formula Search|over-parameterized search]], this gives the best known 8th-order product formulas.

## Complexity

Processor $P$ adds a one-time overhead of typically $M_P$ stages at the start and end. In Morales et al. (2025), the processor for their best 8th-order kernel has 9 free parameters (10 stages of $S_2$). The kernel has 17 stages vs. 21 for the best non-processed formula — saving ~19% per step.

## Caveat

The processor $P$ is not unitary in general — it's a product formula applied once, not guaranteed to be a good approximation by itself. Also, for controlled-time evolution (as in [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|phase estimation]]), the kernel-processor structure requires either modifying the control scheme or absorbing $P$ into the state preparation/measurement.

## Related notes
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Over-Parameterized Product Formula Search]]
- [[Eigenvalue Error as the Correct Product Formula Metric]]
