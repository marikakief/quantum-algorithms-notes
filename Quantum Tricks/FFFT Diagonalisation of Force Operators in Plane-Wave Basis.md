
> **Source:** O'Brien, Streif, Rubin, Santagati, Su, Huggins et al., arXiv:2111.12437
> **Tags:** #trick #plane-wave #FFFT #force-estimation #block-encoding #first-quantized

## What it does

In a plane-wave basis, the nuclear force operator $dH/dR_A$ is strictly one-body (no Pulay forces) and is simultaneously diagonalised by the [[Fermionic Fast Fourier Transform (FFFT)|FFFT]] (second quantisation) or the QFT with aliased frequencies (first quantisation). All force operators on all nuclei commute and share the same diagonal basis.

## The trick

In a plane-wave basis, the overlap matrix is $S_{pq} = \delta_{pq}$ — basis functions don't depend on nuclear positions. The only nuclear-position-dependent part of the Hamiltonian is the electron-nuclear Coulomb term. Differentiating:

$$\frac{dH}{dR_{l,\alpha}} = -\frac{4\pi i Z_l}{\Omega} \sum_{p \neq q} \frac{k_{q-p}^\alpha\, e^{ik_{q-p}\cdot R_l}}{|k_{q-p}|^2}\, a_p^\dagger a_q$$

This is a one-body operator with coefficients that depend on the momentum transfer $q - p$. Apply the [[Fermionic Fast Fourier Transform (FFFT)|FFFT]] (which maps momentum-space creation operators to position-space operators via $a_s^\dagger = \frac{1}{\sqrt{N}} \sum_p c_p^\dagger e^{-ik_s \cdot r_p}$):

$$\frac{dH}{dR_{l,\alpha}} = \sum_p \left(\sum_{s \neq 0} f(s, l)\, e^{ik_s \cdot (R_l + r_p)}\right) c_p^\dagger c_p$$

The result is **diagonal** in position space — a number operator $c_p^\dagger c_p$ weighted by a position-dependent sum. Since different force operators $dH/dR_l$ and $dH/dR_{l'}$ are both diagonal in the same FFFT basis, they all commute. In first quantisation, the same diagonalisation holds with the QFT (aliased frequencies) acting on each electron register.

The block-encoding cost is $T_F = O(\eta \log N + \log N \log(N/\varepsilon))$, with rescaling factor $\lambda_{F_{l,\alpha}} \in O(\eta N^{2/3}/\Omega^{2/3})$ — the same asymptotic scaling as the Hamiltonian's $\lambda_H$.

## When to reach for it

- Plane-wave quantum chemistry simulations (periodic systems, condensed matter) where forces or other first-order energy derivatives are needed.
- First-quantised algorithms ([[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry et al. 2021]]) where the FFFT/QFT is already part of the circuit — force estimation requires only modifying the diagonal phase.
- Simultaneous estimation of forces on all nuclei, since all force operators share the same diagonal basis (no additional measurement overhead from non-commutativity).

## Complexity

Block encoding: $T_F = O(\eta \log N + \log N \log(N/\varepsilon))$ Toffolis. Rescaling: $\lambda_F \in O(\eta N^{2/3}/\Omega^{2/3})$. The QFT on a single electron register costs $O((\log N)^2)$.

## Caveat

Only works for plane-wave bases. Atomic-centred bases (Gaussians) have nuclear-position-dependent overlap matrices, producing Pulay force terms and two-body contributions that break the one-body diagonal structure. Also, the $O(\eta N^{2/3}/\Omega^{2/3})$ scaling of $\lambda_F$ means that for plane waves the force operator is *not* cheaper to block-encode than the Hamiltonian (unlike in second-quantised local bases, where $\lambda_F \ll \lambda_H$). The advantage is structural simplicity and mutual commutativity, not reduced cost.

## Related notes
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]]
- [[Fermionic Fast Fourier Transform (FFFT)]]
- [[First-Quantized Plane-Wave Chemistry Encoding]]
- [[Plane-Wave Dual Basis]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]]
