
> **Source:** Minh C. Tran, Yuan Su, Andrew M. Childs, and Nathan Wiebe, *Faster Digital Quantum Simulation by Symmetry Protection*, arXiv:2006.16248, PRX Quantum **2**, 010323 (2021)  
> **Links:** [arXiv](https://arxiv.org/abs/2006.16248) · [PRX Quantum](https://doi.org/10.1103/PRXQuantum.2.010323)  
> **Tags:** #trick #symmetry #trotter #error-suppression

## Idea

If $H$ commutes with a unitary $C$ (a symmetry), conjugating any simulation step $S_{\delta t}$ by $C$ leaves the ideal evolution unchanged but rotates the error:
$$
C^\dagger S_{\delta t} C = e^{-iH\delta t}(I + C^\dagger M C)
$$
where $M$ is the multiplicative error of the unprotected step. Using a cycle of different $C_k$'s, the rotated errors $\{C_k^\dagger M C_k\}$ can cancel — the ideal signal is invariant, the error gets averaged over symmetry-conjugated directions.

## Why it works

The ideal evolution $e^{-iH\delta t}$ is fixed by every element of the symmetry group; the error $M$ generically is not. A cycle of conjugations effectively projects $M$ onto the subspace that commutes with the symmetry, which can be much smaller than the full error.

## When to use it

- Target model has cheap, explicit symmetries ($\mathbb{Z}_2$, $U(1)$, translation symmetry, etc.).
- Digital simulation error is coherent enough that averaging helps (dominant over stochastic noise).
- Symmetry gates are cheap relative to the simulation step.

## What to watch

The commuting condition $[H, C_k] = 0$ must hold exactly (or very accurately). If the symmetry is approximate, the method introduces its own errors. The symmetry gates must not be too expensive — if each $C_k$ costs as much as the [[Trotter Commutator-Scaling Bound|Trotter]] step, the overhead can negate the gain.

## Related notes

- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]]
- [[Deterministic Symmetry Cycle for Leading-Error Cancellation]]
- [[Randomized Symmetry Protection Between Trotter Steps]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
