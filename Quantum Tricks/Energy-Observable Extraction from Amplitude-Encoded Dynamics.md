
> **Tags:** #trick #observables #amplitude-estimation #simulation
> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, *Exponential quantum speedup in simulating coupled classical oscillators*, arXiv:2303.13012, PRX **13**, 041041 (2023)

## What it does
Reads out kinetic/potential energy fractions directly from subspace/projector probabilities of an [[Standard-Form Encoding (Prepare + Signal Oracle)|amplitude-encoded]] simulation state.

## The trick
Design state amplitudes to carry $\sqrt{m}v$ and $\sqrt{\kappa}\Delta x$, then estimate projector expectation values.

## When to reach for it
- Tasks focused on aggregate energetic observables rather than full trajectory output

## Complexity
Avoids expensive full-state tomography; compatible with amplitude-estimation speedups.

## Caveat
Not suitable when detailed per-coordinate output is required.

## Related Paper Notes
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes]]
