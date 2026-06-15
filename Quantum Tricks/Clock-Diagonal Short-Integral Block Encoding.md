# Clock-Diagonal Short-Integral Block Encoding

> **Source:** Simon, Berry, and Somma, arXiv:2605.16195
> **Tags:** #trick #block-encoding #LCU #matrix-equations #differential-equations

## What it does

Replaces a long Duhamel integral by repeated short-window integrals selected by a clock register.

## The trick

For the matrix ODE solution

$$
\int_0^t ds\,e^{(t-s)A^\dagger}Ce^{(t-s)B},
$$

choose $M$ with $h=t/M\leq 1/\mu$ and define

$$
I_C=\int_0^h d\tau\,e^{\tau A^\dagger}Ce^{\tau B}.
$$

Then

$$
\int_0^t ds\,e^{(t-s)A^\dagger}Ce^{(t-s)B}
=
\sum_{m=0}^{M-1} e^{hmA^\dagger}I_Ce^{hmB}.
$$

So the source term can be inserted into a history-state overlap by the clock-diagonal operator

$$
I=\sum_{m=0}^{M-1}|m\rangle\!\langle m|\otimes I_C+\cdots.
$$

The short integral has a clean two-sided Taylor expansion:

$$
\widetilde I_C=
\sum_{p,q=0}^{K}
\frac{(A^\dagger)^p C B^q}{p!q!}\frac{h^{p+q+1}}{p+q+1}.
$$

That is directly compatible with [[Block-Encoding Composition Algebra]]: powers of $A^\dagger$ and $B$ sandwich a single call to $C$.

## When to reach for it

Use it when a continuous-time inhomogeneous term has the form “left propagator, source, right propagator” and you want to merge it into a clock-state or LCU construction.

## Complexity

For $h\leq \min\{1/a,1/b\}$, the Taylor cutoff is

$$
K=O\!\left(\frac{\log(\|C\|h/\epsilon_{\mathrm{trunc}})}{\log\log(\|C\|h/\epsilon_{\mathrm{trunc}})}\right).
$$

The block-encoding uses $O(K)$ queries to $U_A^\dagger$ and $U_B$, one query to $U_C$, and polylogarithmic extra gates in the block-encoding precision.

## Caveat

The trick relies on choosing the step size small enough that the short-time Taylor expansion is cheap. If $h\|A\|$ or $h\|B\|$ is large, the cutoff grows and the clean bound disappears.

## Related notes

- [[Efficient Quantum Algorithm for Linear Matrix Differential Equations and Applications to Open Quantum Systems (Simon-Berry-Somma 2026) — Paper Notes]]
- [[Two-Sided History-State Overlap for Matrix ODE Entries]]
- [[Sandwich LCU for Matrix Equations]]
- [[Block-Encoding Composition Algebra]]
- [[Taylor-Series-to-Linear-System Encoding]]
