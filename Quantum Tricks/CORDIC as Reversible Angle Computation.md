# CORDIC as Reversible Angle Computation

> **Source:** Burge, Barbeau, Garcia-Alfaro, arXiv:2411.14434 (2024)
> **Tags:** #trick #reversible-computation #quantum-arithmetic #CORDIC #arcsine

## What it does
Computes $\arcsin(t)$ on a quantum computer using only bit-shifts and additions, encoding the result not as a fixed-point number but as a sequence of direction bits that can directly drive controlled rotations.

## The trick

Instead of computing $\theta = \arcsin(t)$ and storing it in a register, CORDIC produces $n-1$ direction bits $d_1, \ldots, d_{n-1}$ such that:

$$\arcsin(t) \approx \sum_{j=1}^{n-1} (-1)^{d_j} \cdot 2\arctan(2^{-j})$$

Each $d_j$ is extracted by iteratively rotating a 2D vector toward the goal direction $(\sqrt{1-t^2}, t)$. The direction decision at step $j$ uses only sign bits — two Toffolis, two CNOTs, and a temporary subtraction.

The pseudo-rotations use only additions of right-shifted registers (no multiplications). The accumulated stretch from pseudo-rotations is compensated by [[Fibonacci-Accelerated Reversible Multiplication|Fibonacci-accelerated multiplication]] of the target vector.

**For amplitude loading:** The direction bits drive controlled-$R_y$ rotations directly:

$$R'|d\rangle|0\rangle = |d\rangle\left[\cos\left(\tfrac{\pi}{4} + \sum_j (-1)^{d_j}\arctan(2^{-j})\right)|0\rangle + \sin(\cdots)|1\rangle\right]$$

using the identity $\arcsin(\sqrt{x}) = \frac{\arcsin(2x-1)}{2} + \frac{\pi}{4}$.

**Reversibility:** The forward CORDIC pass produces garbage in the $x$, $y$, and mult registers. Running the inverse pass (skipping swaps and angle updates) uncomputes this garbage, leaving only the direction bits. After using the direction bits for rotations, a second forward+inverse pass cleans them up.

## When to reach for it
- You need the actual arcsine value (or a rotation by arcsine) and can't use the [[Inequality-Test Amplitude Transduction|comparator trick]]
- Implementing the eigenvalue rotation step in [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] when [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]-based methods aren't applicable
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Quantum Monte Carlo]] amplitude loading where the function $\Phi$ is computed reversibly and then needs amplitude transduction
- Any quantum circuit requiring reversible evaluation of inverse trig functions at moderate precision

## Complexity
For $n$ bits of precision:
- **Qubits:** $5n - 1$ (registers for $t$, $x$, $y$, mult: $n$ each; direction bits: $n-1$; output: 1)
- **Additions:** $< 14n$ total
- **CNOT count:** $O(n^2)$
- **Depth:** $O(n \log n)$ with parallel addition

Compare to [[Inequality-Test Amplitude Transduction]]: $n$ Toffolis and $2n+1$ qubits for amplitude loading specifically.

## Caveat
- For the specific task of encoding stored coefficients into amplitudes, the [[Inequality-Test Amplitude Transduction|inequality-test comparator]] is orders of magnitude cheaper. CORDIC only wins when you genuinely need the arcsine function, not just its effect on amplitudes.
- The $5n-1$ qubit footprint is significant.
- Best suited for moderate precision ($n \lesssim 32$). At higher precisions, polynomial approximation methods may have better asymptotic constants.
- The direction-bit representation means the angle is never materialised as a single fixed-point number — this is a feature for amplitude loading but a limitation if you need the angle for other arithmetic.

## Related notes
- [[Quantum CORDIC — Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024) — Paper Notes]] — source paper
- [[Fibonacci-Accelerated Reversible Multiplication]] — the multiplication subroutine used for stretch compensation
- [[Inequality-Test Amplitude Transduction]] — the much cheaper alternative when arcsine can be avoided
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]] — the paper that showed arcsine is usually unnecessary
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — HHL, where arcsine rotation appears
