
> **Source:** Dominic W. Berry, Andrew M. Childs, Aaron Ostrander, and Guoming Wang, *Quantum algorithm for linear differential equations with exponentially improved dependence on precision*, arXiv:1701.03684, Commun. Math. Phys. **356**, 1057–1081 (2017)  
> **Links:** [arXiv](https://arxiv.org/abs/1701.03684) · [CMP](https://doi.org/10.1007/s00220-017-3002-y)  
> **Tags:** #trick #taylor #numerics #QLSA

## Idea

Choose the number of time segments and the Taylor truncation order together so that [[Order-Condition Cancellation in Product Formulas|local truncation error]], global error accumulation, and conditioning all stay under control.

## Why it matters

You do not get good complexity from Taylor methods by pushing one parameter to the extreme. Tiny [[Step-Size-Taylor-Order Balancing for ODE Simulation|step-size–Taylor-order balancing]] can inflate constants and conditioning. The useful regime comes from balancing both at once.

## When to use it

Use this in Taylor-based quantum simulation or [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|ODE]]-encoding constructions where both discretization and truncation are free design parameters.

## Related notes

- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Taylor-Series-to-Linear-System Encoding]]
