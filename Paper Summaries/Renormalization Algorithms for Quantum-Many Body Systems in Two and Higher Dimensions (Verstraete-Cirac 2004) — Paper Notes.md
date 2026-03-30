> **Source:** Frank Verstraete and J. Ignacio Cirac, *Renormalization algorithms for quantum-many body systems in two and higher dimensions*, arXiv:cond-mat/0407066, 2004
> **Links:** [arXiv:cond-mat/0407066](https://arxiv.org/abs/cond-mat/0407066)
> **Tags:** #tensor-networks #PEPS #variational #2D-systems #many-body #classical-simulation #area-law

---

## The computational problem

Given a 2D (or higher-dimensional) lattice of $N$ quantum spins with local interactions, compute ground-state properties, correlation functions, and time-dependent dynamics. The core challenge: unlike 1D, where [[MPS Canonical Form for Efficient State Tracking|MPS]]/[[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes|TEBD]]/DMRG work well, there was no scalable variational ansatz for higher-dimensional systems.

---

## What the paper does

Introduces **Projected Entangled Pair States (PEPS)** — the natural extension of MPS to 2D and higher-dimensional lattices. Each site on a square lattice has four auxiliary systems (up, down, left, right) of dimension $D$, connected to neighbours via maximally entangled pairs. A local projector $Q_{h,v}$ maps the auxiliary space to the physical space at each site.

The paper also provides algorithms for:
1. **Computing expectation values** of PEPS (via iterative MPS approximation of contracted rows)
2. **Variational ground-state search** (optimise one tensor at a time, reducing to a generalised eigenvalue problem)
3. **Time evolution** (Trotter step + variational truncation back to fixed $D$)
4. **Imaginary time evolution** for ground-state preparation

The key conceptual advance: PEPS naturally obey area-law entanglement scaling — the entanglement entropy of a block is proportional to its boundary, not its volume. This is exactly what's expected for ground states of gapped local Hamiltonians in any dimension, making PEPS the right ansatz for 2D physics in the way MPS is the right ansatz for 1D.

---

## The algorithm / construction

### PEPS definition

For a square lattice with $N = N_h \times N_v$ physical systems of dimension $d$, each site $(h,v)$ has four auxiliary systems $a_{h,v}, b_{h,v}, c_{h,v}, d_{h,v}$ of dimension $D$. Neighbouring auxiliary systems are in maximally entangled states $|\phi\rangle = \sum_{n=1}^{D} |n,n\rangle$ (represented as bonds in the tensor network). A local projector $Q_{h,v}$ maps the 4 auxiliary systems to the physical system:

$$|\Psi\rangle = \sum_{s_{1,1},\ldots,s_{N_h,N_v}} F_2(\{A^{s_{h,v}}_{h,v}\}) |s_{1,1},\ldots,s_{N_h,N_v}\rangle$$

where the tensor $A^s_{h,v}$ has elements $(A^s_{h,v})_{u,d,l,r} = \langle s | Q_{h,v} | u,d,l,r \rangle$ and $F_2$ contracts all indices according to the lattice bonds.

### Why MPS fails in 2D

If you flatten a 2D lattice into a 1D chain (say by snaking through the rows), nearest-neighbour 2D interactions become long-range in the 1D ordering. Worse, the MPS entanglement entropy for a block of sites is bounded by $2\log_2 D$ regardless of block size. In 2D, physical systems obey area-law scaling: a block's entanglement is proportional to its boundary length. MPS can't capture this — you'd need $D$ exponentially large in the boundary length.

PEPS resolves this: the entanglement of a block is proportional to the number of bonds cut, which scales as the boundary area. A block connected to the rest by $b$ bonds has entropy $\leq b \log_2 D$.

### Computing expectation values

For an operator $O = \prod_{h,v} O_{h,v}$, define the four-index transfer tensor:

$$(E_{O_{h,v}})_{\tilde{u},\tilde{d},\tilde{l},\tilde{r}} = \sum_{s,s'} \langle s | O_{h,v} | s' \rangle (A^s)_{u,d,l,r} (A^{s'})^*_{u',d',l',r'}$$

where indices are doubled: $\tilde{u} = (u,u')$, etc. The expectation value reduces to contracting a 2D network of these $E$ tensors.

The contraction is done row by row. Each row can be expressed as a **matrix product operator** (MPO). The first and last rows are MPS. The problem: contracting an MPO with an MPS increases the bond dimension. After $k$ rows, the effective bond dimension is $D^{2k}$ — exponential.

### Variational MPS compression

The solution: after each row contraction, approximate the result by a new MPS of fixed bond dimension $D_f$. This is done by minimising $\||\psi_A\rangle - |\psi_B\rangle\|^2$ over the compressed MPS tensors $\{B^s_k\}$, one site at a time. Since the cost is quadratic in each tensor (with others fixed), each step reduces to solving a linear system. Sweeping back and forth converges to the optimal approximation.

### Variational ground-state search

Minimise $E = x^\dagger \hat{H}_{\text{eff}} x / (x^\dagger \hat{N} x)$ where $x$ contains the components of one tensor $A^s_{\bar{h},\bar{v}}$. This is a generalised eigenvalue problem. Both $\hat{H}_{\text{eff}}$ and $\hat{N}$ (the normalisation matrix) are computed via the row-by-row contraction method. Iterate over all sites until convergence.

### Time evolution

Apply a Trotter step (decomposing the 2D nearest-neighbour Hamiltonian into four sublayers — one for each bond direction). Each sublayer increases the bond dimension by a factor $n$ (the number of terms in the local interaction's tensor product decomposition: $n = 2$ for Ising, $n = 4$ for Heisenberg). After each sublayer, truncate back to bond dimension $D$ via variational optimisation (sweeping row by row, site by site).

---

## Key results

### Numerical benchmarks (2D Heisenberg antiferromagnet)

**$4 \times 4$ lattice** (exact solution available for comparison):
- $D = 2$: relative energy error $1 - E_{\text{var}}/E_{\text{exact}} = 0.02$
- $D = 3$: error $= 0.004$
- $D = 4$: error $= 0.0008$ (essentially exact for this lattice)
- Imaginary time evolution with $D = 3$ is nearly indistinguishable from exact

**$10 \times 10$ lattice:** Fast convergence of imaginary time evolution, with energy improving from $D = 2$ to $D = 3$. Also tested with a frustrated Heisenberg model (some antiferromagnetic bonds replaced by ferromagnetic), where convergence is slower due to smaller gap but still well-controlled.

### Scaling

The number of variational parameters scales as $D^4$ per site (vs. $D^2$ for MPS), so modest $D$ already captures local 2-body density operators accurately. Computational cost is polynomial in $N$ and $D$.

---

## Comparison with prior work

| Method | Dimension | Strengths | Limitations |
|---|---|---|---|
| DMRG / MPS | 1D | Near-exact ground states, dynamics via [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes\|TEBD]] | Fails to scale in 2D; entropy bounded by $2\log D$ independent of block size |
| QMC | Any | Accurate for many systems | Sign problem for frustrated systems |
| 2D DMRG (MPS on flattened lattice) | 2D (forced) | Works for narrow strips | Long-range interactions in 1D ordering; doesn't capture area-law scaling |
| Tensor product states (Nishino et al.) | 2D | Precursor to PEPS for specific models | Restricted to special states; no general variational algorithms |
| **PEPS (this paper)** | **Any** | **Natural area-law ansatz; variational algorithms for ground state and dynamics** | **Contraction is approximate; cost scales as $D^{4+}$ per site** |

---

## Limits / caveats

- **Contraction is approximate:** Unlike 1D MPS where contraction is exact in polynomial time, contracting a 2D PEPS network is #P-hard in general. The row-by-row MPS approximation introduces controlled but non-zero error. The approximation quality depends on $D_f$ (the compression dimension), which must be tuned.

- **Cost scaling with $D$:** The computational cost grows rapidly with bond dimension — roughly $O(D^{10})$ or worse for the full variational optimisation in 2D. This limits practical $D$ to modest values (2–8 in typical applications as of 2004).

- **No rigorous error bounds:** Unlike DMRG in 1D, there are no proven error bounds for PEPS variational energies. The method is variational (so it gives an upper bound on the ground-state energy), but the quality of the bound depends on the representability of the true ground state within the PEPS ansatz at the chosen $D$.

- **Critical systems:** Like MPS, PEPS at fixed $D$ can't exactly represent states with logarithmically divergent entanglement. For 2D critical systems, the area-law violation is typically weak (multiplicative logarithmic corrections), so PEPS handles them better than MPS handles 1D critical systems, but it's still a limitation.

---

## Reusable ideas

1. [[Projected Entangled Pairs as Variational Ansatz]] — Replace the 1D chain of MPS tensors with a 2D network where each site has auxiliary bonds in all lattice directions, connected via maximally entangled pairs. The bond dimension $D$ controls the ansatz expressiveness, and entanglement scales with boundary area automatically.

2. [[Row-by-Row MPS Contraction for 2D Tensor Networks]] — Contract a 2D tensor network by treating each row as an MPO and iteratively applying it to an MPS representation of the accumulated rows, compressing after each step. Trades exact contraction (which is #P-hard) for a controlled approximation.

3. [[Variational Tensor Optimisation via Site-by-Site Sweeping]] — Optimise a tensor network state one tensor at a time: fix all others, reduce the energy to a quadratic function of the free tensor's components, solve the resulting eigenvalue problem, then sweep. This is the PEPS generalisation of the DMRG sweep.

---

## References within this paper

- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal (2003)]] and [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes|Vidal (2004)]] — MPS framework and TEBD that PEPS generalises
- AKLT (Affleck-Kennedy-Lieb-Tasaki 1988) — the 1D valence bond state that MPS generalises; PEPS further generalises to 2D
- Fannes-Nachtergaele-Werner (1992) — finitely correlated states / MPS mathematical framework
- Östlund-Rommer (1995) — MPS as DMRG fixed points
- White (1992) — DMRG
- Verstraete-Porras-Cirac (2004, cond-mat/0404706) — MPS representation where every state can be written as MPS with sufficient $D$; the construction that PEPS builds on
- Vidal-Latorre-Rico-Kitaev (2003) — entanglement scaling in 1D; logarithmic growth at critical points explains why DMRG struggles there
- Nishino et al. (1999, 2000, 2004) — tensor product states for 2D, the precursor to PEPS

---

## Cross-links

### Paper notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — MPS framework; PEPS is the 2D generalisation
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]] — TEBD; PEPS time evolution extends this to 2D
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]] — entanglement and simulability; PEPS obeys area-law scaling
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]] — uses PEPS and MPS as target states for dissipative state engineering
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — uses PEPS as one of the classical heuristics to benchmark against quantum algorithms
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]] — cluster states are a special case of PEPS

### Trick cards
- [[Projected Entangled Pairs as Variational Ansatz]]
- [[Row-by-Row MPS Contraction for 2D Tensor Networks]]
- [[Variational Tensor Optimisation via Site-by-Site Sweeping]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Entanglement as Simulation Complexity Measure]]
