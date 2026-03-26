# Rotation Catalysis via Hamming Weight Phasing

> **Source:** Kivlichan, Gidney, Babbush et al., arXiv:1902.10673, Quantum **4**, 296 (2020); building on Gidney, Quantum **2**, 74 (2018)
> **Tags:** #trick #fault-tolerant #T-gate-reduction #rotation-catalysis #FFFT #circuit-optimization

## What it does

Catalyzes pairs of $\sqrt{T} = T^{1/2} = R_z(\pi/8)$ gates using only 5 T gates and a reusable seed state $\sqrt{T}|+\rangle$, avoiding full rotation synthesis for these specific angles.

## The trick

Standard rotation synthesis for $R_z(\pi/8)$ costs $\sim 20$–$30$ T gates (depending on target precision). But if you have a resource state $\sqrt{T}|+\rangle$ available, you can *catalyze* the rotation: use the resource state as a catalyst that enables the rotation via gate teleportation, consuming only T gates (not $\sqrt{T}$ gates) in the process. The catalyst is recovered after use.

Specifically, for the [[Fermionic Fast Fourier Transform (FFFT)]] with side lengths $\leq 16$, the butterfly rotations are all of the form $R_z(m\pi/2^k)$ with $k \leq 4$. The $k=3$ case ($\pi/8$) requires $\sqrt{T}$. With catalysis, each pair of such rotations costs 5 T gates instead of $\sim 40$–$60$ T gates from synthesis.

Combined with [[Hamming Weight Phasing for Equiangular Rotations]], you first reduce $n$ equiangular $\sqrt{T}$ rotations to $\log n$ rotations on a Hamming weight register, then catalyze those $\log n$ rotations.

## When to reach for it

- FFFT-based Trotter steps for plane-wave systems with side lengths $\leq 16$ (where $\sqrt{T}$ angles appear)
- Any fault-tolerant circuit with many $R_z(\pi/8)$ or $R_z(\pi/16)$ rotations
- When rotation synthesis dominates the T budget and exact angles are known at compile time

## Complexity

- **Cost per pair:** 5 T gates + 1 reusable seed state
- **Savings:** $\sim 8$–$12\times$ vs rotation synthesis per rotation
- **Seed state:** $\sqrt{T}|+\rangle$ prepared once via distillation; reused across all catalysis rounds

## Caveat

Only applies to rotations at specific angles ($\pi/2^k$ and multiples). General-angle rotations still require synthesis. The seed state preparation adds a fixed overhead that's amortized over many uses — for very small circuits, it may not be worth it.

## Related notes
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — source paper
- [[Hamming Weight Phasing for Equiangular Rotations]] — complementary technique (reduce count first, then catalyze)
- [[Fermionic Fast Fourier Transform (FFFT)]] — primary circuit where these angles appear
