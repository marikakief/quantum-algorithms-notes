> **Source:** Zhiyan Ding, Chi-Fang Chen, and Lin Lin, *Single-ancilla ground state preparation via Lindbladians*, arXiv:2308.15676, Phys. Rev. Research **6**, 033147 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2308.15676) · [PRR](https://doi.org/10.1103/PhysRevResearch.6.033147)
> **Tags:** #ground-state-preparation #Lindbladian #open-systems #early-fault-tolerant #Monte-Carlo #single-ancilla #dissipative

---

## The problem

Prepare the ground state $|\psi_0\rangle$ of a Hamiltonian $H$ with spectral gap $\Delta = \lambda_1 - \lambda_0 > 0$, on an early fault-tolerant quantum computer. Specifically: given access to [[Hamiltonian simulation]] $e^{\pm i H t}$ and a simple coupling operator $A$, prepare a state $\rho_{\text{out}}$ with $\|\rho_{\text{out}} - |\psi_0\rangle\langle\psi_0|\|_1 \leq \epsilon$.

Unlike [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|QPE/QSVT-based methods]], this algorithm:
- Uses only **one ancilla qubit** (plus system qubits)
- Requires no initial-state overlap $\gamma = \langle\psi_0|\phi_0\rangle$ — it can work from **zero overlap**
- Needs only mid-circuit measurement/reset of the single ancilla and [[Hamiltonian simulation]]
- Is a quantum analogue of classical Markov chain Monte Carlo

The cost is measured primarily by $T_{H,\text{total}}$, the total Hamiltonian simulation time (sum of all evolution times across all circuit executions).

---

## What the paper does

Constructs a Lindbladian $\mathcal{L}$ whose unique fixed point is $|\psi_0\rangle\langle\psi_0|$, then efficiently simulates this Lindbladian using a single ancilla qubit. The Lindbladian has one jump operator $K$ that annihilates the ground state (by construction: $K|\psi_0\rangle = 0$) and drives population toward lower energy eigenstates. This is a quantum analogue of the Metropolis-Hastings algorithm: the filter function $\hat{f}(\omega)$ enforces detailed balance by zeroing out transitions that increase energy.

The key technical contributions:

1. **Single-ancilla dilation** (Lemma 3): approximate $e^{\mathcal{L}_K \tau}[\rho]$ to $O(\tau^2)$ error by evolving $|0\rangle\langle 0| \otimes \rho$ under a dilated Hermitian operator $\tilde{K}$ for time $\sqrt{\tau}$, then tracing out the ancilla.
2. **Efficient circuit** (Proposition 5): telescoping cancellation of back-and-forth Hamiltonian evolution reduces the total simulation time per step to $O(S_s)$ where $S_s = \Theta(\Delta^{-1} \cdot \text{polylog})$.
3. **Convergence analysis** under ETH-type assumptions (Theorem 7, Theorem 26): the Lindbladian has a unique fixed point and polynomial mixing time.

**My assessment:** This is a genuinely different approach to ground-state preparation — not a refinement of QPE or variational methods, but a quantum MCMC strategy. The single-ancilla requirement is attractive for early fault-tolerant hardware. The main limitation is the cost: the total [[Hamiltonian simulation]] time scales as $\tilde{O}(e^{\Theta(T^2/\epsilon)})$ for the continuous-time version or $\tilde{O}(e^{\Theta(T^{1+1/p}/\epsilon^{1/p})})$ for the $p$-th order discrete version. This is much worse than [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin-Tong (2020)]] or [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|QET-U]] in asymptotic terms, but the hardware requirements are radically simpler. The mixing time — which determines $T$ — remains the big unknown and is likely system-dependent, analogous to classical MCMC.

---

## The algorithm / construction

### The Lindbladian

The continuous-time dynamics is:

$$\frac{d\rho}{dt} = \mathcal{L}[\rho] = \underbrace{-i[H, \rho]}_{\mathcal{L}_H[\rho]} + \underbrace{K\rho K^\dagger - \frac{1}{2}\{K^\dagger K, \rho\}}_{\mathcal{L}_K[\rho]}$$

with a single jump operator:

$$K = \int_{-\infty}^{\infty} f(s)\, e^{iHs} A\, e^{-iHs}\, ds = \sum_{i,j} \hat{f}(\lambda_i - \lambda_j)\, |\psi_i\rangle\langle\psi_i| A |\psi_j\rangle\langle\psi_j|$$

where $A$ is a Hermitian coupling operator and $f(s)$ is the inverse Fourier transform of a filter $\hat{f}(\omega)$ satisfying:

$$\hat{f}(\omega) = 0 \quad \text{for all } \omega \geq 0$$

This condition ensures $K|\psi_0\rangle = 0$ (no upward transitions from the ground state), making $|\psi_0\rangle\langle\psi_0|$ a fixed point. The filter $\hat{f}$ is constructed as a product $\hat{f}(\omega) = \hat{u}(\omega/S_\omega)\hat{v}(\omega/\Delta)$ of Gevrey-class functions (Assumption 12) — compactly supported, rapidly decaying Fourier transform, with $\text{supp}(\hat{v}) \subset (-\infty, 0]$.

### Step 1: Single-ancilla dilation of the dissipative part

Define the dilated Hermitian operator on ancilla + system:

$$\tilde{K} = \begin{pmatrix} 0 & K^\dagger \\ K & 0 \end{pmatrix}$$

**Lemma 3:** For short time $\tau$,

$$\left\|\text{Tr}_a\left[e^{-i\tilde{K}\sqrt{\tau}}(|0\rangle\langle 0| \otimes \rho)\, e^{i\tilde{K}\sqrt{\tau}}\right] - e^{\mathcal{L}_K \tau}[\rho]\right\|_1 = O(\|K\|^4 \tau^2)$$

This is the core insight: evolving the dilated system for time $\sqrt{\tau}$ and tracing out the ancilla approximates the Lindbladian channel to first order in $\tau$, using only one ancilla qubit. Compare this to [[Stinespring Dilation for Open-System Simulation]], which requires a full environment register.

### Step 2: Discretise the jump operator

Approximate $K$ by quadrature:

$$K_s = \sum_{l=-M_s}^{M_s} f(s_l)\, e^{iH s_l} A\, e^{-iH s_l}\, w_l$$

with trapezoid weights, $s_l = l\tau_s$, total support $S_s = M_s \tau_s = \Theta(\Delta^{-1} \cdot (\log(\cdot))^\alpha)$, and step size $\tau_s = \Theta(1/(1 + \max\{\|H\|, S_\omega\}))$.

The dilated operator then factorises as:

$$\tilde{K}_s = \sum_{l=-M_s}^{M_s} \tilde{H}_l, \quad \tilde{H}_l = \sigma_l \otimes A(s_l)$$

where $\sigma_l = w_l(\sigma_x \text{Re}\, f(s_l) + \sigma_y \text{Im}\, f(s_l))$ acts on the single ancilla qubit.

### Step 3: Trotter splitting and cancellation

Apply a second-order Trotter formula to $e^{-i\sqrt{\tau}\tilde{K}_s}$:

$$\overrightarrow{\prod_l} e^{-i\frac{\sqrt{\tau}}{2}\tilde{H}_l} \cdot \overleftarrow{\prod_l} e^{-i\frac{\sqrt{\tau}}{2}\tilde{H}_l}$$

Each factor $e^{-i\frac{\sqrt{\tau}}{2}\sigma_l \otimes A(s_l)}$ decomposes as $(I \otimes e^{iHs_l}) \cdot \tilde{A}_l(\sqrt{\tau}) \cdot (I \otimes e^{-iHs_l})$ where $\tilde{A}_l$ involves only the simple operator $A$ and the ancilla.

**Proposition 5 (Cancellation):** Consecutive Hamiltonian evolutions telescope: $e^{-iHs_l} \cdot e^{iHs_{l+1}} = e^{iH\tau_s}$, reducing the per-step [[Hamiltonian simulation]] cost from $O(M_s \cdot S_s)$ to $O(S_s)$.

The resulting quantum channel $W(\tau)$ (Eq. 16) implements one step of the dissipative evolution. The full iteration is:

$$\rho_{m+1} = e^{\mathcal{L}_H \tau} \circ W(\tau)[\rho_m]$$

The quantum circuit (Figure 2 in the paper) uses the single ancilla qubit, short Hamiltonian evolution segments $e^{\pm iH\tau_s}$, and simple controlled-$A$ gates.

---

## Key results

### Theorem 18 (Continuous-time simulation cost)

For stopping time $T$ and accuracy $\epsilon$:

$$T_{H,\text{total}} = \tilde{\Theta}\left((1 + \|H\|)\Delta^{-1} T^2 \epsilon^{-1}\right)$$

$$N_{A,\text{gate}} = \tilde{\Theta}\left((1 + \|H\|)(1 + \max\{S_\omega, \|H\|\})\Delta^{-1} T^2 \epsilon^{-1}\right)$$

(Using the paper's $\tilde{\Theta}$ notation where $f = \tilde{\Theta}(g)$ means $f = \Theta(g \cdot \text{polylog}(g))$.)

### Theorem 19 (Discrete-time simulation cost)

With $\tau = O(\|A\|^{-2})$ and Trotter parameter $r$:

$$T_{H,\text{total}} = \tilde{\Theta}\left(\Delta^{-1} T^{3/2} \epsilon^{-1/2}\right)$$

### Corollary 20 ($p$-th order Trotter)

Using a $p$-th order Trotter formula:

$$T_{H,\text{total}} = \tilde{\Theta}\left(T^{1+1/p} \epsilon^{-1/p}\right)$$

As $p \to \infty$, this approaches **near-linear in $T$**. This is the best version.

### Theorem 7 (Ergodicity under random coupling)

Under Assumption 6 (random matrix elements of $A$ in the energy eigenbasis, with ETH-type independence), $|\psi_0\rangle\langle\psi_0|$ is the **unique fixed point** in expectation: $\lim_{t\to\infty} \mathbb{E}[\text{Tr}(O\rho(t))] = \langle\psi_0|O|\psi_0\rangle$.

### Theorem 26 (Polynomial mixing time)

Under Assumption 6 plus a spectral cascade condition (Eq. E3) ensuring sufficient transition rates between energy bands, there exists $T^\star = O(\text{poly}(n))$ such that the population in the low-energy subspace (spanned by the first $R_L$ eigenstates) reaches $\Omega(1)$ for $t > T^\star$.

If additionally the sub-transition matrix restricted to the low-energy sector has polynomial mixing, then the ground-state overlap converges to $\Omega(1)$ in $t_{\text{mix}} = O(\text{poly}(n))$.

---

## Comparison with other methods

| Feature | This paper (Lindbladian) | [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes\|Lin-Tong (2020)]] (QSVT) | [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes\|QET-U]] (Dong-Lin-Tong 2022) | QPE |
|---|---|---|---|---|
| Ancilla qubits | **1** | $O(\log(1/\epsilon))$ | 1–3 | $O(\log(1/\epsilon))$ |
| Initial overlap needed | **No** ($p_0 = 0$ ok) | Yes ($\gamma > 0$) | Yes ($\gamma > 0$) | Yes ($\gamma > 0$) |
| Total evolution time | $\tilde{O}(T^{1+1/p}\epsilon^{-1/p})$ where $T = t_{\text{mix}}$ | $\tilde{O}(\alpha\Delta^{-1}\gamma^{-1}\log(1/\epsilon))$ | $\tilde{O}(\Delta^{-1}\gamma^{-1}\epsilon^{-1})$ (short depth) | $O(\Delta^{-1}\gamma^{-1}\epsilon^{-1})$ |
| Key hardness parameter | Mixing time $t_{\text{mix}}$ | Overlap $\gamma$ | Overlap $\gamma$ | Overlap $\gamma$ |
| Multi-qubit control | **None** | Yes (block encoding) | Minimal (Toffoli for near-optimal) | Yes |
| Mid-circuit measurement | Yes (ancilla reset) | No | No | Yes |

The major advantage is operational simplicity: one ancilla, no multi-qubit controlled operations, no initial-state overlap requirement. The major cost is that $t_{\text{mix}}$ is generally unknown and could be exponential. But the same is true of classical MCMC — and classical MCMC works well in practice for many systems despite worst-case guarantees.

---

## Limits / caveats

- **Mixing time is the elephant in the room.** The algorithm works if and only if the Lindbladian mixes in polynomial time. The paper proves this under ETH-type assumptions (Theorem 26), but these are strong and system-dependent. For generic non-commuting Hamiltonians, bounding Lindbladian mixing time remains a hard open problem.
- **Cost scaling is much worse than QSVT-based methods** in the $T$ and $\epsilon$ dependence. Even the best version (Corollary 20, $p \to \infty$) gives $\tilde{O}(T)$ total evolution time, but $T$ itself is $t_{\text{mix}}$ which could be polynomially large.
- **The uniqueness result (Theorem 7) is only in expectation** over random choices of $A$. For a fixed, deterministic $A$, other fixed points may exist and the algorithm can get stuck. The paper's numerics show this: without the coherent $\mathcal{L}_H$ term, the algorithm sometimes converges to wrong fixed points.
- **The $2 \times 2$ non-normal example** in the paper (where $Q$ is exponential in Fang-Lin-Tong's framework) is suggestive — the Lindbladian approach may handle non-normal dynamics more naturally — but this isn't formalised.
- **Circuit depth per step** scales with $S_s = \Theta(\Delta^{-1} \cdot \text{polylog})$, which determines the longest coherent evolution required. For small-gap systems this is already substantial on early fault-tolerant hardware.

---

## Reusable ideas

1. [[Single-Ancilla Lindbladian Dilation for Dissipative Simulation]] — approximate the Lindbladian channel $e^{\mathcal{L}_K \tau}$ to first order by evolving a $2\times 2$ dilated Hermitian operator for time $\sqrt{\tau}$ on one ancilla qubit, then tracing out.

2. [[Frequency-Domain Filter for Energy-Monotone Quantum Jumps]] — construct a jump operator $K$ that annihilates the ground state by choosing a filter $\hat{f}(\omega)$ with $\hat{f}(\omega) = 0$ for $\omega \geq 0$, ensuring only downward energy transitions.

3. [[Telescoping Hamiltonian Evolution Cancellation in Trotter Products]] — when a Trotter product involves alternating forward/backward Hamiltonian evolutions $e^{\pm iH s_l}$, consecutive segments cancel telescopically, reducing per-step cost from $O(M \cdot S)$ to $O(S)$.

---

## References within this paper

- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin & Tong (2020)]] — near-optimal ground state preparation via QSVT eigenstate filtering [44]
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin & Tong (2022)]] — Heisenberg-limited energy estimation, early fault-tolerant [45]
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|Dong, Lin & Tong (2022)]] — QET-U ground state preparation [27]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low & Wiebe (2019)]] — QSVT framework [32]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — QPE [38]
- Temme, Osborne, Vollbrecht, Poulin & Verstraete, *Quantum Metropolis sampling*, Nature (2011) — quantum Metropolis [68]
- Cubitt, *Dissipative ground state preparation and the dissipative quantum eigensolver*, arXiv:2303.11962 (2023) — dissipative eigensolver, related approach [21]
- Chen, Kastoryano, Brandão & Gilyén, *Quantum thermal state preparation*, arXiv:2303.18224 (2023) — quantum Gibbs sampling [15]
- Chen & Brandão, *Fast thermalization from ETH*, arXiv:2112.07646 (2023) — ETH-based convergence for open quantum systems [12]
- Cleve & Wang, *Efficient quantum algorithms for simulating Lindblad evolution*, ICALP 2017 — Lindbladian simulation complexity [20]
- Polla, Herasymenko & O'Brien, *Quantum digital cooling*, PRA (2021) — related dissipative cooling approach [56]

---

## Cross-links

### Paper notes
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — QSVT-based competitor
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]] — early fault-tolerant competitor
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]] — same group, energy estimation
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — polynomial eigenvalue transformations used in competitors
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]] — spectral gap as a resource for ground state preparation
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — variational alternative
- [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes]] — heuristic Hamiltonian-switching approach; unlike this paper, changes $H$ rather than adding jump operators; both avoid eigenstate stalling
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]] — the foundational paper on dissipative quantum computation and state engineering; this work extends beyond frustration-free Hamiltonians

### Trick cards
- [[Single-Ancilla Lindbladian Dilation for Dissipative Simulation]] — new trick card
- [[Frequency-Domain Filter for Energy-Monotone Quantum Jumps]] — new trick card
- [[Telescoping Hamiltonian Evolution Cancellation in Trotter Products]] — new trick card
- [[Stinespring Dilation for Open-System Simulation]] — general dilation; this paper's approach is a special case using only one ancilla
- [[Entropy Pumping for State Preparation]] — related idea of entropy removal via ancilla reset
- [[QET-U — Eigenvalue Transformation via Hamiltonian Evolution]] — alternative eigenvalue-transformation approach for ground state prep
