# Postselection-Cost Parameterization via g-Factor

> **Tags:** #trick #postselection #complexity #linear-odes
> **Source:** arXiv:1701.03684

## What it does
Makes success-amplification overhead explicit through a physically meaningful growth/decay ratio parameter.

## The trick
Track $g=\max_t\|x(t)\|/\|x(T)\|$ in complexity to separate intrinsic dynamical attenuation from algorithmic overhead. This cost is related to the [[Oblivious Amplitude Amplification (Robust)|OAA]] overhead needed to boost the final output amplitude.

## When to reach for it
- Algorithms where target output amplitude is naturally small at final time
- [[Taylor-Series-to-Linear-System Encoding|Taylor-to-linear-system]] reductions where the ODE has significant decay

## Complexity
Clarifies unavoidable amplification cost and guides reformulation/preconditioning.

## Caveat
Large $g$ can dominate runtime regardless of precision improvements.

## Related Paper Notes
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
