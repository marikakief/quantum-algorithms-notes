
> **Source:** Andrew M. Childs, Robin Kothari, and Rolando D. Somma, *Quantum algorithm for systems of linear equations with exponentially improved dependence on precision*, arXiv:1511.02306, SIAM J. Comput. **46**, 1920–1950 (2017)  
> **Links:** [arXiv](https://arxiv.org/abs/1511.02306) · [SIAM J. Comput.](https://doi.org/10.1137/16M1087072)  
> **Tags:** #trick #chebyshev #QLSA #polynomials

## Idea

Approximate a regularized version of the inverse function on the relevant spectral interval by a Chebyshev polynomial, then implement that polynomial transform as an operator algorithm.

## Why it matters

This is the polynomial side of the post-HHL shift away from phase estimation. It shows that linear-systems solving can be attacked by approximation theory directly, which is exactly the line of thought that later becomes standard in [[QSVT Meta-Template|QSVT]]-based algorithms.

## When to use it

Use this when the operator model makes polynomial transforms natural and you want a more explicit approximation-theoretic route than the Fourier-integral [[Linear Combination of Unitaries (LCU)|LCU]] construction.

## Caveat

You have to regularize the singularity near zero and control the degree needed on the conditioned spectral interval, so the approximation problem is delicate rather than automatic.

## Related notes

- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Fourier Integral LCU for Matrix Inversion]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
