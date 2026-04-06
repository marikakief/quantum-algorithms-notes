> **Source:** Andrew M. Childs, *On the relationship between continuous- and discrete-time quantum walk*, Commun. Math. Phys. **294**, 581–603 (2010); arXiv:0810.0312
> **Links:** [arXiv](https://arxiv.org/abs/0810.0312) · [CMP](https://doi.org/10.1007/s00220-009-0930-1)
> **Tags:** #quantum-walk #Hamiltonian-simulation #qubitization-precursor #QSP-precursor #walk-simulation #foundational

---

## What the paper does

Three things that turned out to be deeply connected:

1. **Unifies continuous- and discrete-time quantum walks:** Gives an exact correspondence via Szegedy's quantization — the first general proof that continuous-time walk is a limit of discrete-time walk.

2. **Linear-time [[Hamiltonian simulation]] via walks:** Simulates $e^{-iHt}$ using $O(\|H\|_{\mathrm{abs}} t/\sqrt{\delta})$ steps of a discrete-time walk. This is **linear** in $t$, beating the $t^{3/2}$ of direct Trotter and the $t \cdot \mathrm{polylog}$ of [[Product Formulas]]s. Works for **non-sparse** Hamiltonians.

3. **Seeds quantum signal processing:** The spectral relationship $\lambda \mapsto e^{\pm i\arcsin\lambda}$ between Hamiltonian eigenvalues and walk eigenphases is exactly the structure that Low and Chuang later exploited for [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] and [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]].

---

## The correspondence: Szegedy quantization of Hamiltonians

### Construction

Given an $N \times N$ Hermitian matrix $H$ with $|d\rangle$ the principal eigenvector of $\mathrm{abs}(H)$ (entrywise absolute value), define states:

$$
|\psi_j\rangle = \frac{1}{\sqrt{\|\mathrm{abs}(H)\|}} \sum_k \sqrt{\frac{H^*_{jk}}{d_k} \cdot d_j} |j, k\rangle
$$

These are orthonormal, and form an isometry $T = \sum_j |\psi_j\rangle\langle j|$.

The discrete-time quantum walk is:

$$
U = iS(2TT^\dagger - I)
$$

where $S$ is the swap $|j,k\rangle \leftrightarrow |k,j\rangle$.

### The spectral theorem (Theorem 1)

If $\frac{H}{\|\mathrm{abs}(H)\|}|\lambda\rangle = \lambda|\lambda\rangle$, then $U$ has eigenvectors $|\mu_\pm\rangle$ with eigenvalues:

$$
\mu_\pm = \pm e^{\pm i\arcsin\lambda}
$$

**This is the key relationship:** Hamiltonian eigenvalues $\lambda$ map to walk eigenphases $\pm\arcsin\lambda$. The $\arcsin$ is the fingerprint of this approach — it reappears in [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] and is the function that [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSP/QSVT]] generalises to arbitrary polynomials.

---

## Continuous-time walk as a limit

### The lazy walk

To make all eigenvalues small (so $\arcsin\lambda \approx \lambda$), introduce a laziness parameter $\epsilon$:

$$
|\psi_j^\epsilon\rangle = \sqrt{\epsilon}\,|\psi_j\rangle + \sqrt{1-\epsilon}\,|\bot_j\rangle
$$

This replaces $\lambda$ with $\epsilon\lambda$ in Theorem 1, so the walk eigenphases become $\pm\arcsin(\epsilon\lambda) \approx \pm\epsilon\lambda$.

### The limit (Theorem 2)

$$
\left\|T^\dagger \frac{1-iS}{\sqrt{2}} (iU)^\tau \frac{1+iS}{\sqrt{2}} T - e^{-i\tau H/\|\mathrm{abs}(H)\|}\right\| \leq h^2\left(1 + (\tfrac{\pi}{2}-1)h\tau\right)
$$

where $h = \|H\|/\|\mathrm{abs}(H)\|$. The rotation by $(1 \pm iS)/\sqrt{2}$ projects out the unwanted $+$ branch of the $\pm e^{\pm i\arcsin\lambda}$ eigenvalue pairs.

**Complexity:** $\tau = O((ht)^{3/2}/\sqrt{\delta})$ walk steps to simulate $e^{-iHt}$ to error $\delta$.

---

## Linear-time simulation via phase estimation

The $t^{3/2}$ scaling from the direct limit can be improved to **linear** using [[Gapped Phase Estimation|phase estimation]]:

1. Apply isometry $T$: $|\psi\rangle \mapsto T|\psi\rangle = \sum_\lambda \psi_\lambda T|\lambda\rangle$
2. Phase estimate on $U$ to extract $\arcsin\lambda$
3. Apply $e^{-i(\sin\phi)t}$ (where $\phi$ is the estimated phase — and $\sin\phi \approx \lambda$)
4. Uncompute phase estimation
5. Apply $T^\dagger$

**Theorem 5:** Simulate $e^{-iHt}$ with fidelity $\geq 1 - \delta$ using $O(\|\mathrm{abs}(H)\| t / \sqrt{\delta})$ walk steps.

Using optimal phase estimation (sine window, not uniform superposition) achieves $O(\|\mathrm{abs}(H)\| t / \sqrt{\delta})$ — **linear in $t$**, optimal by the no-fast-forwarding theorem.

---

## Applications

### Non-sparse [[Hamiltonian simulation]]

The walk-based simulation doesn't require sparsity — only that the walk step (reflection about $\mathrm{span}\{|\psi_j\rangle\}$ + swap) can be implemented efficiently. This requires:
- Index-computability: given vertex $j$, list its neighbors
- Degree-computability: given $j$, compute $\deg(j)$

For regular graphs or graphs with efficiently computable structure, this works even when the degree is polynomial in $N$ (non-sparse).

### Continuous-time element distinctness

The discrete-time element distinctness algorithm ([[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis 2007]]) can now be converted to a continuous-time version, answering the open question from the survey literature.

### Optimal simulation of continuous-time query algorithms

Any continuous-time quantum walk algorithm making $T$ oracle calls can be simulated in the standard discrete-time query model with $O(T)$ queries.

---

## The road to QSP and QSVT

This paper is the **missing link** between quantum walks and quantum signal processing:

| This paper (2010) | Low-Chuang qubitization (2019) | Gilyén et al. QSVT (2019) |
|---|---|---|
| $U = iS(2TT^\dagger - I)$ | $U = (\langle G| \otimes I)(c\text{-}e^{i\phi\sigma_z})(|G\rangle \otimes I)$ | General signal processing conventions |
| Eigenvalues $\pm e^{\pm i\arcsin\lambda}$ | Same spectral structure | Arbitrary polynomial $P(\lambda)$ via phase factors |
| Phase estimation to extract $\lambda$ | Phase factors to implement $P(\lambda)$ | Polynomial transformations of singular values |
| Linear-time simulation | Optimal simulation + any bounded polynomial | Universal framework |

The $\arcsin$ relationship is the foundation. Qubitization asks: instead of extracting $\lambda$ via phase estimation and then applying a function, can we directly implement $P(\lambda)$ by interleaving walk steps with phase rotations? QSP/QSVT answers yes.

---

## The sign problem

The paper identifies a limitation: when $H$ has negative off-diagonal entries, $\|\mathrm{abs}(H)\| > \|H\|$ and the simulation cost depends on $\|\mathrm{abs}(H)\|$ rather than $\|H\|$. This **sign problem** means the simulation can be polynomially less efficient than the norm would suggest.

For the glued-trees Hamiltonian (Childs-Farhi-Gutmann), $\|\mathrm{abs}(H)\|$ is exponentially larger than $\|H\|$, so the walk-based simulation doesn't preserve the exponential speedup. This motivated subsequent work on handling signs.

---

## Reusable ideas

1. **[[Szegedy Walk Quantization of Hamiltonians]]:** Map any Hermitian $H$ to a discrete-time walk $U = iS(2TT^\dagger - I)$ with eigenvalue relationship $\lambda \mapsto \pm e^{\pm i\arcsin\lambda}$. The walk operates on $\mathbb{C}^N \otimes \mathbb{C}^N$, with the isometry $T$ encoding the Hamiltonian structure. This is the bridge between quantum walks, [[Hamiltonian simulation]], and quantum signal processing.

2. **[[Linear-Time Hamiltonian Simulation via Walk Phase Estimation]]:** Combine Szegedy quantization with phase estimation: extract $\arcsin\lambda$ from the walk, apply $e^{-it\sin\phi}$, uncompute. Achieves $O(\|\mathrm{abs}(H)\| t / \sqrt{\delta})$ complexity — linear in $t$, works for non-sparse Hamiltonians.

---

## References within this paper

- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — defined the walk quantization framework; Theorem 1 of this paper derives from Szegedy's spectral theorem
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — discrete-time element distinctness; this paper gives the continuous-time version
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs & Goldstone (2004)]] — continuous-time search on lattices
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — survey posing the open question of continuous/discrete relationship
- Berry, Ahokas, Cleve & Sanders (2007) — sparse [[Hamiltonian simulation]] via [[Product Formulas]]s
- Childs, Cleve, Deotto, Farhi, Gutmann & Spielman (2003) — exponential speedup by continuous-time walk (glued trees)
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — [[Gapped Phase Estimation|phase estimation]] used for the linear-time simulation

---

## Cross-links

### Paper notes
- [[Universal Computation by Quantum Walk (Childs 2009) — Paper Notes]] — clarifies how the continuous-time walk model used for universality sits relative to discrete-time walk constructions
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — directly builds on the $\arcsin$ spectral relationship; replaces phase estimation with QSP phase factors
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — generalises to arbitrary polynomial transformations of block-encoded matrices
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — the walk quantization framework this paper extends
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — the discrete-time algorithm that gets a continuous-time version here
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — develops the simulation further

### Trick cards
- [[Szegedy Walk Quantization of Hamiltonians]]
- [[Linear-Time Hamiltonian Simulation via Walk Phase Estimation]]
- [[Coined Quantum Walk Search on Graphs]]
- [[Gapped Phase Estimation]]
- [[Continuous-Time Quantum Walk Search]]
