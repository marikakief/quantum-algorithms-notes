

> **Tags:** #trick #amplitude-amplification #condition-number #qlsa
> **Source:** arXiv:1511.02306 (Childs-Kothari-Somma); original technique: Andris Ambainis, "Variable time amplitude amplification and quantum algorithms for linear algebra problems," STACS 2012, doi:10.4230/LIPIcs.STACS.2012.636

## What it does

Improves the condition-number dependence in quantum linear systems algorithms from $O(\kappa^2)$ (HHL) toward $\tilde{O}(\kappa)$ by partitioning the eigenvalue spectrum into regimes and doing different amounts of work per regime, then amplifying globally.

## The trick

The idea is that eigenvectors with eigenvalue $\lambda$ close to 0 (i.e., small eigenvalues, which control the large singular values of $A^{-1}$) require more simulation time to resolve and invert accurately than eigenvectors with $|\lambda| \sim 1$. Rather than running the algorithm for the worst-case (smallest eigenvalue) time for all components, variable-time [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]] lets different branches halt at different times based on their eigenvalue regime.

Concretely: partition the spectral interval into $O(\log\kappa)$ dyadic buckets. Run the inversion algorithm for a time proportional to $1/|\lambda|$ for each bucket. Apply [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]] globally across all branches simultaneously.

The overall result is $\tilde{O}(\kappa)$ query complexity rather than $O(\kappa^2)$, at the cost of a more complex branched circuit structure and additional ancilla registers for the clock.

## When to reach for it

- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|QLSA]] settings where $\kappa$-quadratic cost is the bottleneck
- Any algorithm with a similar structure: variable query cost across a spectrum of problem instances

## Complexity

Moves from $O(\kappa^2/\epsilon)$ (HHL with [[Gapped Phase Estimation|phase estimation]]) toward $\tilde{O}(\kappa \log(1/\epsilon))$ when combined with the Fourier/Chebyshev precision improvement.

## Caveat

More complex control flow, additional ancillas for the branching clock, and nontrivial overhead from the simultaneous amplification. The asymptotic gain is real but the constants can matter in practice.

## Related Paper Notes
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Quantum Linear Matrix Equations — Paper Notes]]
