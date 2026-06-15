
> **Source:** Berry, Rubin, Babbush et al., arXiv:2312.07654
> **Tags:** #trick #block-encoding #pseudopotential #design-principle #QROM #arithmetic #scaling

## What it does

An architectural principle for block encoding operators with known analytical structure: compute the operator's functional form via coherent arithmetic ($O(\text{polylog}\, N)$ cost) rather than tabulating it via [[QROM (Quantum Read-Only Memory)|QROM]] ($O(N)$ or $O(\text{poly}(N))$ cost). Applied to the GTH pseudopotential, this gives a roughly 100× improvement in the paper's block-encoding comparisons over the QROM-heavy approach. The tradeoff is that breaking the analytic expression into cheaper pieces can inflate the LCU normalization $\lambda$, increasing the number of phase-estimation steps.

## The trick

The GTH pseudopotential has a complicated but *analytically known* form: exponentials times polynomials of momentum, with parameters depending on nuclear species and angular momentum quantum numbers. There are two ways to handle this in a block encoding:

**QROM approach (Shokrian Zini et al. 2023):** Tabulate the pseudopotential values (or Grover-Rudolph state preparation amplitudes) over the entire plane-wave basis. The QROM must output data proportional to the basis size $N$, giving $O(N)$ Toffoli cost just for the pseudopotential. For non-diagonal angular momentum projectors, the data grows further.

**Arithmetic approach (this paper):** Use the analytical form. The expensive part — the Gaussian exponential — is computed once via [[QROM Interpolation for Negative Exponential|QROM interpolation]] (cost ~256 entries, independent of $N$). The polynomials $c_{0,lj} + c_{1,lj}(r^\alpha_l \|k\|)^2 + c_{2,lj}(r^\alpha_l \|k\|)^4$ are computed via a handful of multiplications (each $O(b^2)$). The angular-momentum-dependent Legendre polynomials $P_l(\hat{k}_p \cdot \hat{k}_q)$ are computed by combining dot products and norms that are already available.

The total arithmetic cost scales as $O(b^2)$ per evaluation, where $b \sim 20$ is the number of precision bits. Since $b = O(\log N)$ in the address-register model, this replaces an $O(N)$ table with an $O(\log^2 N)$ arithmetic calculation as a function of the table address size. It is not a complexity-theoretic exponential speedup in the physical problem size.

What makes this work:
1. The GTH pseudopotential has a **separable** structure: the exponential depends on $\|k\|^2$ only, not on the direction; the angular dependence is through low-degree Legendre polynomials; the species-dependent parameters are discrete (loaded via small QROM).
2. The **expensive function** (exponential) appears only **once** in each term — it's shared between the local and nonlocal parts via [[Shared Exponential Evaluation Across Pseudopotential Terms|the $q = 0$ trick]].
3. The **species-dependent data** (nuclear positions, pseudopotential parameters) is $O(L)$ where $L$ is the number of atoms — loaded via QROM that scales with $L$, not $N$.

## When to reach for it

- Any block encoding where the operator has a known analytical form (screened potentials, Gaussian-type basis functions, trigonometric structure)
- When the alternative is tabulating function values over a computational basis that grows with system size
- The principle applies beyond pseudopotentials: any time you'd reach for QROM to encode a smooth function, ask whether coherent arithmetic + interpolation would be cheaper
- *Not* applicable when the operator is truly unstructured (arbitrary numerical matrix elements with no analytical form)

## Complexity

| Approach | Pseudopotential cost | Scaling with $N$ |
|---|---|---|
| QROM (Shokrian Zini et al.) | $O(N)$ Toffolis | Linear |
| Arithmetic (this paper) | $O(b^2) \sim O(\log^2 N)$ Toffolis | Polylogarithmic |

For realistic systems: the arithmetic approach costs ~5,000–10,000 Toffolis for the pseudopotential evaluation, compared to the controlled swaps ($12\eta n \sim 15,000$–$40,000$) that dominate the full block encoding in the paper's examples. QROM/QROAM costs can be space-time traded, and arithmetic costs include precision and finite-interval constants; the table captures the main scaling contrast, not every constant.

## Caveat

The arithmetic approach trades Toffoli cost for $\lambda$ inflation. To keep the arithmetic tractable, the sum over angular momentum indices $l, i, j$ is broken up in PREPARE (each term prepared separately), which means $\lambda$ accumulates the *sum of absolute values* of individual terms rather than the *absolute value of the sum*. For transition metals (Ni, Mn) with $l_{\max} = 2$ and three projectors per $l$, this can inflate $\lambda_{\text{nonloc}}$ by a significant factor.

This is the classic block-encoding tradeoff: simpler circuits at the cost of larger $\lambda$, meaning more QPE steps. Whether the tradeoff is worthwhile depends on the relative costs.

Rule of thumb: arithmetic wins when analytic structure and moderate precision make $\operatorname{poly}(b)$ arithmetic cheaper than QROM/QROAM table lookup, and when the induced $\lambda$ increase is tolerable in the total phase-estimation cost.

## Related notes
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — The paper articulating this principle
- [[QROM (Quantum Read-Only Memory)]] — The alternative data-loading approach
- [[QROM Interpolation for Negative Exponential]] — The specific interpolation technique for the exponential
- [[Shared Exponential Evaluation Across Pseudopotential Terms]] — Complementary trick for sharing computation
