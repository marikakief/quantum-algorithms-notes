# Binary-Decomposed SELECT for Parameter-Dependent Unitaries

> **Source:** Dong An, Jin-Peng Liu, and Lin Lin, arXiv:2303.01029; see also Childs-Kothari-Somma (2017), An-Fang-Lin (2022)
> **Tags:** #trick #LCU #SELECT-oracle #binary-decomposition

## What it does
Constructs a SELECT oracle $\sum_{j=0}^{M} |j\rangle\langle j| \otimes U_0^{k_j}$ using only $O(\log M)$ queries to controlled versions of $U_0$, when the parameters $k_j$ are evenly spaced.

## The trick
When $U_j = U_0^{k_j}$ for uniformly spaced $k_j = k_0 + j\Delta$, the SELECT oracle factors via the binary representation of $j$. Write $j = \sum_l j_l 2^l$. Then:

$$\mathrm{SEL} = U_0^{k_0} \prod_{l=0}^{\lceil\log M\rceil} c\text{-}U_0^{2^l \Delta}\quad\text{(controlled on bit } l \text{ of register)}$$

Each controlled gate $c\text{-}U_0^{2^l\Delta}$ is a single call to the oracle for $U_0$ at the appropriate parameter value. Total: $O(\log M)$ oracle calls instead of $O(M)$.

In the LCHS context, the unitaries are $e^{-iL(\tau)k_js}$ with $k_j$ evenly spaced. Factor out $e^{iL(\tau)Ks}$ and apply $c\text{-}(e^{-iL(\tau)\cdot 2sK/M})^j$ via binary decomposition.

## When to reach for it
- Any LCU where the unitaries form a geometric or arithmetic family parametrised by an index.
- Quantum simulation with parameter-dependent evolution (e.g., different frequencies, coupling constants, or time intervals).
- LCHS implementations, interaction-picture methods, and continuous-$k$ LCUs after discretisation.

## Complexity
$O(\log M)$ queries to build the SELECT oracle, where $M$ is the number of terms in the LCU.

## Caveat
Requires the $U_j$'s to be *powers* of a common base unitary $U_0$ (or close to it). Non-uniform spacings or qualitatively different unitaries don't decompose this way.

## Related notes
- [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — the LCU framework this builds on
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] — uses similar binary tricks for Fourier LCU
