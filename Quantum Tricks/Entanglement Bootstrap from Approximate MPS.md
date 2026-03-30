# Entanglement Bootstrap from Approximate MPS

> **Source:** Hastings, arXiv:0705.2024
> **Tags:** #trick #area-law #entanglement #MPS #bootstrap

## What it does
Converts an approximate description of the ground state as a low-Schmidt-rank state into a bound on the ground state's entanglement entropy. The entropy bound depends logarithmically on the Schmidt rank and on the inverse overlap.

## The trick
Suppose a density matrix $\rho$ is a mixture of pure states with Schmidt rank $\leq k$ across a cut at site $j$, and has overlap $P = \langle\Psi_0|\rho|\Psi_0\rangle > 0$ with the ground state of a gapped local Hamiltonian. Then:

$$S(\rho_{1,j}^0) \leq \ln(k) + \xi' \ln(2C_1(\xi)^2/P) \ln(D) + F(\xi', D)$$

where $F(\xi', D) = (\xi'+4)\ln(D) + 1 + \ln(D^2 - 1) + \ln(\xi'/2 + 1)$.

The proof works by applying the [[Approximate Ground State Projector from Local Operators|local projector decomposition]] $O_B O_L O_R \approx P_0$ to $\rho$. Each application of $O_B$ (supported on $2m$ sites) can increase the effective Schmidt rank by at most $D^{2m}$, while the overlap with $|\Psi_0\rangle$ is maintained. This gives a sequence of increasingly tight bounds on the Schmidt coefficient tail:

$$\sum_{\alpha \geq kD^{2m}+1} |A_0(\alpha)|^2 \leq 2C_1(\xi)^2 \exp(-2m/\xi')/P$$

Maximising the entropy subject to this tail constraint yields the bound.

The same approach works for Rényi entropies $S_\alpha$ when $e^{-2\alpha/\xi'} D^{2(1-\alpha)} < 1$.

## When to reach for it
- You have a rough approximation to a ground state (say from variational methods or adiabatic continuation) and want to bound the true entanglement
- Proving area laws: start with any bounded-Schmidt-rank state having nonzero overlap, then bootstrap into an entropy bound
- Relating [[MPS Canonical Form for Efficient State Tracking|MPS]] bond dimension to entanglement entropy in gapped systems

## Complexity
The entropy bound is linear in the boundary correlation length $\xi'$ and logarithmic in $k$ and $1/P$. For a practical MPS with bond dimension $k$, this tells you how close the entropy can be to the true value.

## Caveat
The resulting entropy bound is exponential in $\xi'$, inheriting the looseness of the area law. The bootstrap only works because the gap provides the local projector decomposition — it's not a generic property of entangled states. Also, the bound becomes vacuous when $P \to 0$ (logarithmic divergence), so you need meaningful overlap to start.

## Related notes
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]]
- [[Approximate Ground State Projector from Local Operators]]
- [[Entanglement as Simulation Complexity Measure]]
- [[MPS Canonical Form for Efficient State Tracking]]
