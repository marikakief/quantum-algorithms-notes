
> **Source:** Sanders, Berry, Costa, Tessler, Wiebe, Gidney, Neven, Babbush, arXiv:2007.07391
> **Tags:** #trick #qubitization #adiabatic #quantum-walk #heuristic

## What it does
Simulates an adiabatic path using a sequence of [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk steps, where each step advances by a small "time" $\sim 1/r$ without requiring explicit [[Hamiltonian simulation]] circuits. The per-step cost depends on the LCU query complexity, not the number of Hamiltonian terms.

## The trick
A qubitized walk $W(s)$ for Hamiltonian $H(s)$ at adiabatic parameter $s$ encodes eigenvalues as $e^{\pm i \arccos(E_k / \lambda)}$. This corresponds to unit-time evolution under $\arccos(H/\lambda)$ — too large a step for adiabatic refinement.

To shrink the step: inflate the normalization $\lambda \to r\lambda$ by adding identity terms to the LCU decomposition:

$$H = \sum_k \lambda_k U_k \;\;\longrightarrow\;\; \sum_k \lambda_k U_k + \frac{(r-1)\lambda}{2}(\mathbb{1} - \mathbb{1})$$

This changes the PREPARE state to include the extra identity terms, and the SELECT oracle gains one additional control qubit. The resulting walk $W_r(s)$ has eigenvalues encoding $r \arcsin(E_k / (r\lambda))$, which for large $r$ approximates $E_k / \lambda$. Each application advances by time $\sim 1/r$.

A [[product formula]] over adiabatic parameter values $s_1, s_2, \ldots$ gives:

$$\prod_m W_r(s_m) \approx \text{adiabatic evolution along } \{s_m\}$$

The adiabatic path generated is through $\arccos(H(s)/\lambda(s))$ rather than through $H(s)$ directly, but for eigenvalues in the linear regime of $\arccos$ (i.e., away from $\pm \lambda$), this is nearly identical.

## When to reach for it
- Adiabatic state preparation or optimization where the cost function Hamiltonian has many terms but a cheap LCU representation (e.g., LABS: $O(N^3)$ terms but $O(N)$ walk cost).
- When Trotter-based adiabatic evolution is bottlenecked by the number of Hamiltonian terms.
- As a heuristic — the time step doesn't need to be exact for heuristic optimization.

## Complexity
Per walk step: same as the underlying LCU walk (PREPARE + SELECT + reflection), plus $O(1)$ overhead for the extra identity terms. The number of steps needed for $\varepsilon$-accurate adiabatic preparation scales as:

$$\tilde{O}\left(\frac{1}{\varepsilon^{3/2}} \cdot \text{poly}\left(\frac{\|\dot{H}\|, \|\ddot{H}\|, |\dot{\lambda}|, |\ddot{\lambda}|}{\min(\Delta, \min_k |E_k|)^2}\right)\right)$$

Improvable to $1/\varepsilon^{o(1)}$ via higher-order Suzuki splitting and boundary cancellation methods (Kieferová & Wiebe 2014).

## Caveat
The bound depends on $\min_k |E_k|$ (not just the gap $\Delta$), because eigenvalues near zero in the walk have degenerate phases under $W^4$. The formal convergence analysis is loose — practical performance is likely better but unproven. Not asymptotically optimal; exists primarily as a simple, low-overhead alternative to Dyson series approaches for early fault-tolerant machines.

## Related notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — Rigorous version of discrete adiabatic walks applied to QLSP; proves explicit gap-dependent error bounds via the [[Discrete Adiabatic Theorem for Quantum Walks]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Coherent Alias Sampling for PREPARE]]
- [[QROM (Quantum Read-Only Memory)]]
