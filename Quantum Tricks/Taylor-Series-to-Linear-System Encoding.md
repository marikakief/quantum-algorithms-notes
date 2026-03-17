
> **Source:** Dominic W. Berry, Andrew M. Childs, Aaron Ostrander, and Guoming Wang, *Quantum algorithm for linear differential equations with exponentially improved dependence on precision*, arXiv:1701.03684, Commun. Math. Phys. **356**, 1057–1081 (2017)  
> **Links:** [arXiv](https://arxiv.org/abs/1701.03684) · [CMP](https://doi.org/10.1007/s00220-017-3002-y)  
> **Tags:** #trick #linear-odes #QLSA #taylor

## Idea

Encode the truncated Taylor recurrences for time evolution into one sparse block-structured linear system whose solution stores the history of the ODE evolution.

## Why it matters

This is the central reduction in the paper: it converts a dynamics problem into a linear-systems problem, which means improved [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|QLSA]] methods can be used as a subroutine. The algorithmic benefit comes from the reduction, not from a new integrator by itself.

## When to use it

Use this when the evolution law is linear with constant coefficients and you want to trade direct time stepping for one structured solve.

## Caveat

The reduction is only attractive if the resulting linear system remains sparse and well-conditioned enough that the [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|QLSA]] advantage is not swallowed by conditioning and postselection overhead.

## Related notes

- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
