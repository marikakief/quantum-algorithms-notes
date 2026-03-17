
> **Source:** Andrew M. Childs, Robin Kothari, and Rolando D. Somma, *Quantum algorithm for systems of linear equations with exponentially improved dependence on precision*, arXiv:1511.02306, SIAM J. Comput. **46**, 1920–1950 (2017)  
> **Links:** [arXiv](https://arxiv.org/abs/1511.02306) · [SIAM J. Comput.](https://doi.org/10.1137/16M1087072)  
> **Tags:** #trick #LCU #QLSA #fourier

## Idea

Represent the inverse function \(1/x\) on the relevant spectral interval using a Fourier-style integral or sum of oscillatory phase factors, then lift that scalar representation to an operator representation involving evolutions under \(A\).

## Why it matters

This is one of the cleanest pre-[[QSVT Meta-Template|QSVT]] examples of the “approximate the scalar function, then quantize the approximation” philosophy. The inverse is no longer obtained by resolving eigenvalues via phase estimation; it is approximated functionally.

## When to use it

Use this when Hamiltonian-simulation access to \(e^{-iAt}\) is natural and you want an [[Linear Combination of Unitaries (LCU)|LCU]]-based route to approximating \(A^{-1}\).

## Caveat

The coefficient norm and success-amplification overhead are not free, so the representation has to be judged as an algorithmic package, not just as a pretty integral identity.

## Related notes

- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Chebyshev-Regularized Inverse Approximation]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
