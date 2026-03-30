# Half-Filling Reduction from Hubbard to Heisenberg

> **Source:** Schuch and Verstraete, arXiv:0712.0483; Auerbach, *Interacting Electrons and Quantum Magnetism* (1994)
> **Tags:** #trick #Hubbard-model #Heisenberg-model #perturbation-theory #condensed-matter

## What it does
Shows that the Hubbard model at half filling with strong on-site repulsion ($U \gg t$) reduces to the antiferromagnetic Heisenberg model, making the computational complexity of one equivalent to the other.

## The trick
The Hubbard model on a lattice with $N$ sites and $N$ electrons (half filling):

$$H = -t\sum_{\langle i,j\rangle, s} a^\dagger_{i,s}a_{j,s} + U\sum_i n_{i,\uparrow}n_{i,\downarrow}$$

When $U \gg t$, double occupancy of any site costs energy $U$. In the ground state, each site has exactly one electron, providing a spin-$\frac{1}{2}$ degree of freedom.

Virtual hopping processes contribute at second order: an electron on site $i$ hops to neighbouring site $j$ (both now on $j$, costing energy $U$), then one hops back. This only works if the two electrons form a singlet (by the Pauli principle), giving an effective coupling:

$$H_{\text{eff}} = \frac{2t^2}{U}\sum_{\langle i,j\rangle} \vec{\sigma}_i \cdot \vec{\sigma}_j$$

The coupling $J = 2t^2/U > 0$ is antiferromagnetic. Local magnetic fields from the Hubbard model map directly to local fields in the Heisenberg model (first-order contribution).

**Error:** Higher-order processes contribute $O(N^3 t^3/U^2)$, so $U/t$ must grow polynomially with $N$ for polynomial precision.

## When to reach for it
- Complexity-theoretic reductions between Hubbard and Heisenberg models (QMA-completeness transfers in both directions)
- Understanding the magnetic properties of Mott insulators — the Heisenberg model is the effective theory
- Deriving spin Hamiltonians from electronic models in quantum chemistry and materials science
- Justifying why quantum simulation of Hubbard models gives access to spin physics

## Complexity
No computational cost — this is a mapping between models. The reduction is polynomial-time: the Heisenberg Hamiltonian is constructed from the Hubbard parameters in $O(N)$ time.

## Caveat
Only valid at half filling with $U \gg t$. Away from half filling (doped Hubbard model), the physics is qualitatively different — charge degrees of freedom become relevant, and the effective model is a $t$-$J$ model rather than pure Heisenberg. This is precisely the regime relevant to high-$T_c$ superconductivity, where the Hubbard model remains unsolved.

For the complexity reduction, the polynomially large $U/t$ ratio makes the Hamiltonian parameters unrealistic, but this is standard in QMA-completeness proofs.

## Related notes
- [[Computational Complexity of Interacting Electrons and Fundamental Limitations of DFT (Schuch-Verstraete 2009) — Paper Notes]]
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]]
- [[Perturbation Gadgets for Hamiltonian Complexity Reduction]]
