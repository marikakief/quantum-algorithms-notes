> **Source:** Martin Schwarz, Toby S. Cubitt, Kristan Temme, Frank Verstraete, and David Perez-Garcia, *Preparing topological PEPS on a quantum computer*, Physical Review A **88**, 032321 (2013)
> **Links:** [arXiv:1211.4050](https://arxiv.org/abs/1211.4050) · [PRA](https://doi.org/10.1103/PhysRevA.88.032321)
> **Tags:** #PEPS #tensor-networks #topological-order #state-preparation #toric-code #quantum-circuits #Marriott-Watrous

---

## The computational problem

**Input:** A G-injective PEPS defined by local tensors $A_\nu$ on an $N$-vertex lattice, along with the finite group $G$ and its representation.

**Output:** A quantum state in the ground state subspace of the associated parent Hamiltonian.

**Question:** Can G-injective PEPS — which include topologically ordered states like toric code ground states and resonating valence bond (RVB) states — be prepared efficiently on a quantum computer?

---

## What the paper does

Gives a polynomial-time quantum algorithm for preparing G-injective PEPS on a quantum computer, extending the earlier Schwarz-Temme-Verstraete result for injective (non-topological) PEPS. The key challenge is that topological PEPS have degenerate ground state subspaces, which breaks the [[Jordan's Lemma Subspace Decomposition for Alternating Measurements|Marriott-Watrous rewinding trick]] used in the injective case.

The paper overcomes this by proving that the G-symmetry structure of the PEPS ensures that the overlap between successive ground state subspaces is lower-bounded by $\kappa(A_{t+1}|_{S_G})^{-2}$, where $\kappa(\cdot|_{S_G})$ is the condition number restricted to the G-symmetric subspace. This is the key technical lemma (Lemma 2) that makes the whole algorithm work.

Runtime: $O(N^4 \kappa_G^2 \Delta^{-1} \varepsilon^{-1})$ where $\Delta$ is the minimum spectral gap of the parent Hamiltonians and $\kappa_G = \max_t \kappa(A_t|_{S_G})$.

---

## The algorithm

### PEPS background

A PEPS on a square lattice is constructed by:
1. Placing maximally entangled states of dimension $D$ (the bond dimension) on every edge
2. Applying a linear map $A_\nu: (\mathbb{C}^D)^{\otimes 4} \to \mathbb{C}^d$ at each vertex $\nu$

The resulting state on the physical Hilbert space $(\mathbb{C}^d)^{\otimes N}$ is the PEPS. When $A$ is invertible, the PEPS is **injective** and has a unique ground state for its parent Hamiltonian (frustration-free, consisting of local projectors).

### G-injective PEPS

For a finite group $G$ with a semi-regular representation $U_g = \bigoplus_\alpha V_g^\alpha \otimes \mathbb{1}_{r_\alpha}$, define a re-weighting map $\Delta = \bigoplus_\alpha (d_\alpha/r_\alpha)^{1/4} \mathbb{1}$ and the G-isometric tensor:

$$A_\nu = \frac{1}{|G|}\sum_{g \in G} \overline{\Delta U_g} \otimes \overline{\Delta U_g} \otimes \Delta U_g \otimes \Delta U_g$$

A **G-injective** PEPS is obtained by applying an additional invertible map on top of the G-isometric one. The parent Hamiltonian has a degenerate ground space labelled by boundary conditions $K = (g,h)$ with $[g,h] = 0$ — this ground state degeneracy is the hallmark of topological order.

For the regular representation, the G-isometric PEPS parent Hamiltonian is exactly Kitaev's [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes|quantum double model]].

### Algorithm structure

| Step | Operation | Cost |
|---|---|---|
| **1. Prepare G-isometric PEPS** | Use the Aguado-Vidal circuit for quantum double ground states | $O(N\log N)$ |
| **2. For $t = 1$ to $N$:** | Successively project onto ground states of partial parent Hamiltonians $H_t$ | See below |
| **2.1** | Measure $\{P_{t+1}, P_{t+1}^\perp\}$ via quantum phase estimation on $e^{-iH_{t+1}\tau}$ | $\tilde{O}(N^2/\Delta)$ |
| **2.2** | If failed: alternate measurements $\{P_t, P_t^\perp\}$ and $\{P_{t+1}, P_{t+1}^\perp\}$ until success | $O(\kappa_G^2)$ repetitions |

### Why it works: the overlap bound (Lemma 2)

The minimum overlap between the ground state subspaces of $H_t$ and $H_{t+1}$ is:

$$d_{\min} = \min_{|\psi_t\rangle} \max_{|\psi_{t+1}\rangle} |\langle\psi_t|\psi_{t+1}\rangle|^2 \geq \kappa(A_{t+1}|_{S_G})^{-2}$$

This works because:
- Ground states of $H_t$ lie in the G-symmetric subspace $S_G$
- Applying $A_{t+1}$ (positive semidefinite, invertible on $S_G$) to any ground state of $H_t$ produces a state in the ground space of $H_{t+1}$
- The overlap is bounded by the ratio of extreme eigenvalues of $A_{t+1}$ on $S_G$

The key difference from the injective case: we restrict the condition number to the G-symmetric subspace rather than the full space. On the full space $\kappa = \infty$ (because $A$ has zero eigenvalues outside $S_G$), but on $S_G$ it's finite.

### Failure probability

After $m$ back-and-forth measurement steps:

$$p_{\text{fail}}(m) < \frac{1}{2\,d_{\min}\,m}$$

This converges as $1/m$, so $O(N\kappa_G^2/\varepsilon)$ repetitions per site suffice for total success probability $\geq 1 - \varepsilon$.

---

## Key results

**Theorem 4 (Runtime):** A G-injective PEPS on an $N$-vertex lattice can be prepared on a quantum computer with probability $\geq 1 - \varepsilon$ in time $O(N^4 \kappa_G^2 \Delta^{-1} \varepsilon^{-1})$, where $\Delta = \min_t \Delta_t$ is the minimum spectral gap and $\kappa_G = \max_t \kappa(A_t|_{S_G})$.

**Examples of states covered:**
- Toric code ground states (G = $\mathbb{Z}_2$)
- Quantum double model ground states for any finite group
- Resonating valence bond states on the Kagome lattice ($\mathbb{Z}_2$-injective PEPS, with numerical evidence for the gap assumption)
- String-net models and Hopf algebra constructions (extensions mentioned)

---

## Comparison with prior work

| Paper | States prepared | Topological order? | Key technique |
|---|---|---|---|
| Schwarz-Temme-Verstraete (2012), Ref. [5] | Injective PEPS | No | [[Jordan's Lemma Subspace Decomposition for Alternating Measurements\|Marriott-Watrous rewinding]] with unique ground states |
| Aguado-Vidal (2008) | Quantum double ground states | Yes (but specific) | Explicit $O(N\log N)$ circuit |
| **This paper** | G-injective PEPS | Yes (general) | Extended Marriott-Watrous with condition number on G-symmetric subspace |
| Verstraete-Cirac-Latorre (2009) | MPS with bond dimension $D$ | No | Sequential isometries; depth $O(ND)$ |

This paper fills a gap: injective PEPS preparation excludes topological order, and Aguado-Vidal handles only the specific case of quantum doubles. G-injective PEPS are a broad class that covers most known topological tensor network states.

---

## Limits / caveats

- The runtime scales as $\Delta^{-1}$, so a polynomially closing gap makes preparation exponentially expensive. Whether the partial parent Hamiltonians $H_t$ maintain a polynomial gap is model-dependent — the paper requires this as an assumption, not a proof.
- The algorithm prepares *a* state in the ground state subspace, not a specific ground state. For topologically ordered systems this is a feature (you get some superposition of topological sectors), but it means you don't control which anyon sector you end up in.
- The condition number $\kappa_G$ can be large for poorly conditioned PEPS tensors, making the algorithm slow.
- Each step uses quantum phase estimation with runtime $\tilde{O}(N^2/\Delta)$, which already involves [[Hamiltonian simulation]] as a subroutine. The total cost is dominated by the $N^4$ factor from doing $N$ sites with $O(N)$ QPE calls per site.
- The paper focuses on PEPS on a square lattice. Generalisations to other lattices (triangular, Kagome, honeycomb) are straightforward in principle but not explicitly worked out.

---

## Reusable ideas

1. [[Marriott-Watrous Rewinding for Degenerate Ground Spaces]] — Extends the [[Jordan's Lemma Subspace Decomposition for Alternating Measurements|Marriott-Watrous rewinding technique]] from unique ground states to degenerate ground spaces by restricting the condition number to a symmetry subspace. The key insight: even when the full condition number is infinite, the condition number on the relevant symmetric subspace can be finite and well-bounded.

2. [[Sequential PEPS Construction via Ground State Projection]] — Build a many-body state by starting from an easily preparable state and sequentially projecting onto ground states of partially constructed parent Hamiltonians. Each step is implemented via quantum phase estimation + alternating measurements.

---

## References within this paper

- Schwarz-Temme-Verstraete (2012), PRL 101:110502 — predecessor for injective PEPS preparation
- Schuch-Cirac-Perez-Garcia (2010), Annals of Physics 325:2153 — introduces G-injective PEPS and their parent Hamiltonians
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes|Kitaev (2003)]] — toric code and quantum double models; the G-isometric PEPS for the regular representation is exactly the quantum double
- Aguado-Vidal (2008), PRL 100:070404 — polynomial circuit for quantum double ground states; used as step 1 of this algorithm
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes|Marriott-Watrous (2005)]] — the rewinding technique for QMA amplification, adapted here for state preparation
- Jordan (1875) — Jordan's lemma on simultaneous block diagonalisation of two projectors
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov-Ta-Shma (2003)]] — jagged adiabatic lemma; alternative approach if generalised to degenerate case
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo (2013)]] — potential improvement via gap amplification

---

## Cross-links

### Paper notes
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]] — toric code / quantum double models that G-isometric PEPS generalise
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]] — Marriott-Watrous rewinding technique
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]] — potential improvement to the algorithm
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]] — jagged adiabatic lemma alternative
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — MPS simulation; PEPS is the 2D generalisation
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]] — Cubitt's later work on Hamiltonian complexity
- [[Computational Complexity of Interacting Electrons and Fundamental Limitations of DFT (Schuch-Verstraete 2009) — Paper Notes]] — same research group (Verstraete); complexity of ground state problems

### Trick cards
- [[Marriott-Watrous Rewinding for Degenerate Ground Spaces]]
- [[Sequential PEPS Construction via Ground State Projection]]
- [[Jordan's Lemma Subspace Decomposition for Alternating Measurements]]
