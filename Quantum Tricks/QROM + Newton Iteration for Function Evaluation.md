# QROM + Newton Iteration for Function Evaluation

> **Tags:** #trick #arithmetic #function-evaluation #QROM
> **Source:** arXiv:2510.07380 (Quantum FMM, Berry et al.); technique developed in Babbush et al. (plasma simulation papers) and used here for the Coulomb $1/r$ evaluation

## What it does
Evaluates smooth functions (like $1/r$) on quantum registers to precision $\varepsilon$ using table lookup + iterative refinement.

## The trick
1. **QROM lookup**: Load a coarse approximation $f_0(x)$ from a precomputed classical table indexed by the most significant bits of $|x\rangle$. Cost: $O(1)$ Toffolis per table entry using [[Unary Encoding for Sequential Controlled Operations|unary iteration]].

2. **Newton iteration**: Refine $f_0$ by repeating $f_{k+1} = f_k(2 - x \cdot f_k)$ (for $1/x$) or analogous update. Each iteration **doubles** the number of correct bits.

3. After $O(\log\log(1/\varepsilon))$ iterations, precision $\varepsilon$ is reached.

Each Newton step requires one quantum multiplication: $O(\log^2(1/\varepsilon))$ gates.

## When to reach for it
- Computing smooth nonlinear functions ($1/r$, $\sqrt{r}$, $\log r$, etc.) on quantum registers
- When full QROM at target precision would be too large
- Any quantum arithmetic where a coarse table + refinement beats exact computation

## Complexity
$O(\log^2(1/\varepsilon) \log\log(1/\varepsilon))$ per evaluation. QROM table size: $O(2^b)$ where $b$ = bits for initial approximation (typically small).

## Caveat
Requires the function to be smooth and Newton-iterable in the relevant domain. Singularities (like $1/r$ at $r=0$) need cutoff handling.
