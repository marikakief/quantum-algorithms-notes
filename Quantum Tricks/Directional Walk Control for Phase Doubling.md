
> **Source:** Berry, Motlagh, Pantaleoni, Wiebe, arXiv:2401.10321; also Babbush, Gidney, Berry, Wiebe et al., arXiv:1805.03662
> **Tags:** #trick #QSP #GQSP #qubitization #block-encoding #hamiltonian-simulation #phase-doubling

## What it does

Doubles the effective eigenphase accumulated per step in a [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]-style algorithm by replacing control between $\mathbb{1}$ and $U$ with control between $U$ and $U^\dagger$.

## The trick

In standard QSP, a controlled operation selects between the identity and the walk operator $U$. If $U$ has eigenvalue $e^{i\theta}$, the controlled operation implements an effective rotation with eigenvalues $1$ and $e^{i\theta}$. By alternating the control qubit state, this becomes a rotation $\hat{R}(\theta/2)$ with eigenvalues $e^{\pm i\theta/2}$.

Now replace this with a **directionally controlled** operation: control qubit $|0\rangle$ applies $U$, control qubit $|1\rangle$ applies $U^\dagger$. The effective rotation becomes $\hat{R}(\theta)$ with eigenvalues $e^{\pm i\theta}$ — double the phase per step.

After $N$ directionally controlled operations, the accessible polynomial degree is $N$ (not $N/2$ as in standard QSP). The query count for a given target polynomial degree is halved.

For [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]]-based walk operators, flipping $U$ to $U^\dagger$ amounts to moving the ancilla reflection before/after the block-encoding unitary $V$. This is typically free or near-free — just a reordering of existing operations.

## When to reach for it

- Any [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]/[[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]-based algorithm where the block-encoding allows cheap inversion of the walk step.
- [[Hamiltonian simulation]] where $\lambda t$ dominates the query cost and a constant-factor improvement matters (e.g., fault-tolerant chemistry resource estimates).
- Eigenvalue estimation, where [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush et al. (2018)]] already demonstrated the $2\times$ improvement.

## Complexity

Halves the query count from $K+1$ to approximately $K/2 + 2$ controlled operations (plus two boundary corrections) for a degree-$K$ [[Jacobi-Anger Truncation for QSP Simulation|Jacobi–Anger truncation]].

## Caveat

Requires [[Generalized Quantum Signal Processing (GQSP)|GQSP]] (complex polynomial pairs) rather than standard QSP (real polynomials) when applied to [[Hamiltonian simulation]]. The directional control changes the Fourier parity of the accessible polynomials, requiring [[Parity Shifting via Spectral Multiplication in GQSP|parity shifting]] to match the Jacobi–Anger expansion.

Also requires that inverting $U$ is cheap. For qubitization from [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low–Chuang]], this is nearly free. For other block-encoding constructions, the cost may not be negligible.

## Related notes

- [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]] — source paper for the simulation application
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — first use of this trick for eigenvalue estimation
- [[Parity Shifting via Spectral Multiplication in GQSP]] — companion trick for handling the parity mismatch
- [[Jacobi-Anger Truncation for QSP Simulation]] — the approximation scheme that benefits from the doubling
- [[Bessel Linearisation of Quantum Walk Phases]] — earlier walk-phase transformation approach
