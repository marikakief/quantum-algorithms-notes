# Quantum Uploading as One-Time Input Noise

> **Source:** Kannan, Putterman, and Cotler, arXiv:2605.02057
> **Tags:** #trick #fault-tolerance #surface-code #quantum-learning #noise

## What it does

Turns an unencoded quantum state supplied by an experiment into a logical encoded state while charging the physical interface as a one-time input-noise channel. The formal theorem starts after a constant-error experiment-to-memory interface is available; it does not solve arbitrary physical transduction.

## The trick

Suppose a quantum experiment produces an unknown physical state $\rho$ that one wants to process coherently. If the state is processed directly, every nontrivial learning operation can expose it to fresh physical noise. Instead:

1. **Transduce** the physical system into quantum memory.
2. **Inject** each physical qubit into a small surface-code patch.
3. **Grow** the patch to distance $d$ using stabilizer measurement rounds.
4. **Interpret growth faults** as an effective input channel.
5. **Run all later computation fault-tolerantly** on the uploaded logical state.

The resulting abstraction, after twirling / learning-model simplifications, is

$$
\rho \longmapsto V_d^{\otimes n}D_\lambda^{\otimes n}(\rho)(V_d^\dagger)^{\otimes n},
\qquad \lambda=O(p),
$$

up to residual logical error exponentially small in $d$. The formal theorem first gives a general marginal input channel

$$
N_{{\rm inj},i}=(1-q_i)\operatorname{Id}+q_iR_i,
\qquad q_i\le c p,
$$

then uses logical Clifford twirling to make it a depolarising channel for the learning analysis.

The conceptual move is to pay the physical input noise once, then run the later learning computation fault-tolerantly.

## When to reach for it

- Quantum learning from physical samples: photons, spins, molecules, analog simulators.
- Multi-copy protocols such as moment estimation, virtual distillation, or qPCA-like routines.
- Randomized-measurement protocols where the randomizing circuit is deep enough that raw physical noise would dominate.
- Any proof where the distinction between one-time state-preparation noise and repeated processing noise matters.

## Complexity

For the surface-code construction in the paper:

- upload depth: $O(d)$ stabilizer rounds;
- input noise: constant strength $O(p)$ under below-threshold circuit noise;
- residual logical failure: exponentially small in $d$;
- later fault-tolerant simulation overhead: polylogarithmic in circuit size and target accuracy.

## Caveat

The trick assumes the transduction into memory is possible with constant error. If the experiment-to-memory interface destroys the relevant quantum information before injection, fault tolerance has nothing left to protect.

It also shifts cost into architecture. The sample-complexity theorem treats uploaded logical processing abstractly; the physical qubit and decoding overhead may still be large.

The speedup claim is upload-first fault-tolerant quantum processing versus raw noisy quantum processing. It is not a quantum-versus-classical separation.

## Related notes

- [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]]
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]]
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]
- [[Sparse Fault Path Analysis]]
- [[Upload-Then-Randomize for Noisy Shadows]]
