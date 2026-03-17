> **Source:** Dorit Aharonov, Wim van Dam, Julia Kempe, Zeph Landau, Seth Lloyd, Oded Regev, *Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation*, arXiv:quant-ph/0405098, FOCS 2004 / SIAM J. Comput. **37**(1), 166–194 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0405098) · [FOCS](https://doi.org/10.1109/FOCS.2004.8)
> **Tags:** #adiabatic #equivalence #history-state #spectral-gap #hamiltonian-simulation #markov-chains

---

## What the paper does

Proves that adiabatic quantum computation and standard (circuit-model) quantum computation are polynomially equivalent. The hard direction is showing that any quantum circuit can be efficiently simulated by an adiabatic evolution with local Hamiltonians.

To prove the equivalence, the authors build an explicit sparse [[Hamiltonian simulation]] — and along the way introduce or refine several techniques that became standard tools:

- [[History-State Encoding with Unary Clock|Kitaev's history-state construction]] (adapted for adiabatic use)
- [[History-State Encoding with Unary Clock|Unary clock encoding]] with local verification
- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|Hamiltonian-to-Markov-chain mapping]] for spectral gap analysis
- Conductance bounds applied to quantum Hamiltonians
- [[Perturbation Lemma for Locality Reduction|Perturbation theory for locality reduction]] (5-local → 3-local → 2-local)
- [[Identity-Gate Padding for Output Amplification|Identity-gate padding]] for output amplification
- [[Snake-Path 2D Embedding for Nearest-Neighbor Universality|Snake-path 2D embedding]] for nearest-neighbor universality

---

## The computational problem

**Given:** A quantum circuit with $L$ two-qubit gates $U_1, \ldots, U_L$ on $n$ qubits.

**Goal:** Construct local Hamiltonians $H_{\mathrm{init}}$, $H_{\mathrm{final}}$ such that adiabatic evolution from the ground state of $H_{\mathrm{init}}$ to the ground state of $H_{\mathrm{final}}$ produces the circuit's output state $U|0^n\rangle$.

**Running time:** Determined by the spectral gap of $H(s) = (1-s)H_{\mathrm{init}} + s H_{\mathrm{final}}$ via the adiabatic theorem. The paper defines running time as $T \cdot \max_s \|H(s)\|$ (time × energy), which is invariant under rescaling of $H$.

---

## The adiabatic theorem (Theorem 2.1)

For $H(s) = (1-s)H_{\mathrm{init}} + s H_{\mathrm{final}}$ with unique ground state for all $s$, and any fixed $\delta > 0$:

$$
T \geq \Omega\!\left(\frac{\|H_{\mathrm{final}} - H_{\mathrm{init}}\|^{1+\delta}}{\epsilon^\delta \cdot \min_{s} \Delta^{2+\delta}(H(s))}\right)
$$

guarantees the final state is $\epsilon$-close (in $\ell_2$) to the ground state of $H_{\mathrm{final}}$.

The $\delta > 0$ is a technical parameter (think $\delta = 0.1$); the hidden constant diverges as $\delta \to 0$.

**Key implication:** To show efficient adiabatic simulation, it suffices to show that $\Delta(H(s)) \geq 1/\mathrm{poly}(L)$ for all $s$.

---

## The [[History-State Encoding with Unary Clock|history state]] (Kitaev's construction)

The ground state of $H_{\mathrm{final}}$ is not the unknown circuit output $|\alpha(L)\rangle$ — that would require knowing the answer to construct the Hamiltonian. Instead, it is the **history state**:

$$
|\eta\rangle = \frac{1}{\sqrt{L+1}} \sum_{\ell=0}^{L} |\alpha(\ell)\rangle \otimes |1^\ell 0^{L-\ell}\rangle_c
$$

where $|\alpha(\ell)\rangle = U_\ell \cdots U_1 |0^n\rangle$ is the circuit state after $\ell$ gates, and the right register is a **[[History-State Encoding with Unary Clock|unary clock]]** counting steps by adding 1s from left to right.

The history state encodes the entire computation trajectory in superposition. It has overlap $1/\sqrt{L+1}$ with the final state $|\alpha(L)\rangle$, which is enough to extract the output by measuring the clock.

**Why this works:** The history state can be verified locally — each propagation step $|\alpha(\ell-1)\rangle \to |\alpha(\ell)\rangle$ involves only the gate $U_\ell$ and two adjacent clock qubits. The final state $|\alpha(L)\rangle$ alone cannot be verified locally because it depends on the entire circuit.

```
 Computation     Clock (unary)
 register        register

 |α(0)⟩    ⊗    |000...0⟩     ← step 0
 |α(1)⟩    ⊗    |100...0⟩     ← step 1 (U₁ applied)
 |α(2)⟩    ⊗    |110...0⟩     ← step 2 (U₂ applied)
   ⋮              ⋮
 |α(L)⟩    ⊗    |111...1⟩     ← step L (all gates applied)

 History state |η⟩ = equal superposition of all rows
```

---

## The 5-local Hamiltonian construction

```
H_init  = H_clockinit + H_input + H_clock
H_final = (1/2) Σ H_ℓ + H_input + H_clock

H(s) = (1-s) H_init + s H_final
```

Note: $H_{\mathrm{input}}$ and $H_{\mathrm{clock}}$ appear in both, so they are held constant during interpolation. Only $H_{\mathrm{clockinit}}$ is swapped out for $\frac{1}{2}\sum H_\ell$.

### Clock Hamiltonian
Penalizes illegal clock states (any state not of the form $|1^\ell 0^{L-\ell}\rangle$):

$$
H_{\mathrm{clock}} = \sum_{\ell=1}^{L-1} |01\rangle\langle 01|_{c_\ell, c_{\ell+1}}
$$

Forbids the substring "01" in the clock register. Legal states have eigenvalue 0; illegal states have eigenvalue $\geq 1$.

### Input Hamiltonian
Checks that computation qubits are $|0^n\rangle$ when clock reads $|0^L\rangle$:

$$
H_{\mathrm{input}} = \sum_{i=1}^{n} |1\rangle\langle 1|_i \otimes |0\rangle\langle 0|_{c_1}
$$

### Clock initialization
Forces clock to start at $|0^L\rangle$:

$$
H_{\mathrm{clockinit}} = |1\rangle\langle 1|_{c_1}
$$

### Propagation Hamiltonian
For $1 < \ell < L$, checks correct gate application at step $\ell$:

$$
H_\ell = I \otimes |100\rangle\langle 100|_{c_{\ell-1,\ell,\ell+1}} - U_\ell \otimes |110\rangle\langle 100|_{c_{\ell-1,\ell,\ell+1}} - U_\ell^\dagger \otimes |100\rangle\langle 110|_{c_{\ell-1,\ell,\ell+1}} + I \otimes |110\rangle\langle 110|_{c_{\ell-1,\ell,\ell+1}}
$$

This is positive semidefinite (not a projector — its nonzero eigenvalues are 2, since $U_\ell$ is unitary). It penalizes any state where the transition from clock state $\ell-1$ to $\ell$ doesn't match $U_\ell$. The three clock qubits $c_{\ell-1}, c_\ell, c_{\ell+1}$ detect which step the clock is at; the gate $U_\ell$ acts on two computation qubits. Total: 5-local.

**Structure:** Each $H_\ell$ is positive semidefinite, and $H_\ell |\eta\rangle = 0$ because the history state satisfies every propagation check by construction.

---

## Spectral gap analysis

This is the technical heart of the paper. The gap must be $\geq 1/\mathrm{poly}(L)$ for all $s \in [0,1]$.

### Step 1: Identify an invariant subspace

Define $S_0 = \mathrm{span}\{|\gamma_0\rangle, \ldots, |\gamma_L\rangle\}$ where $|\gamma_\ell\rangle = |\alpha(\ell)\rangle \otimes |1^\ell 0^{L-\ell}\rangle$.

$S_0$ is invariant under $H(s)$: the Hamiltonian never takes a state in $S_0$ outside of it. So the adiabatic evolution stays in $S_0$, and it suffices to bound the gap of $H_{S_0}(s)$.

### Step 2: Write the restricted Hamiltonians explicitly

In the basis $\{|\gamma_0\rangle, \ldots, |\gamma_L\rangle\}$:

$$
H_{S_0,\mathrm{init}} = \mathrm{diag}(0, 1, 1, \ldots, 1)
$$

$$
H_{S_0,\mathrm{final}} = \frac{1}{2}\begin{pmatrix} 1 & -1 & & \\ -1 & 2 & -1 & \\ & -1 & 2 & \ddots \\ & & \ddots & \ddots & -1 \\ & & & -1 & 1 \end{pmatrix}
$$

The final Hamiltonian restricted to $S_0$ is exactly the **graph Laplacian of a path on $L+1$ vertices** (the discrete 1D random walk Laplacian), scaled by $1/2$.

### Step 3: Two regimes

**Case $s < 1/3$:** $H_{S_0}(s)$ is close to $H_{S_0,\mathrm{init}}$ (which has gap 1). By **Gershgorin's circle theorem**, one eigenvalue is $< 1/3$ and all others are $> 2/3$. Gap $\geq 1/3$.

**Case $s \geq 1/3$:** Use the [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|Hamiltonian-to-Markov-chain mapping]].

### Step 4: [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|Hamiltonian → Markov chain]] → conductance

Define $G(s) = I - H_{S_0}(s)$. Since $G(s)$ has non-negative entries and satisfies Perron's theorem conditions, its largest eigenvector $(\alpha_0, \ldots, \alpha_L)$ has all positive entries.

Construct stochastic matrix (Equation 3 of the paper):

$$
P_{ij} = \frac{\alpha_j}{\mu \alpha_i} G_{ij}
$$

where $\mu = 1 - \lambda$ is the largest eigenvalue of $G(s)$ and $\lambda$ is the ground energy of $H_{S_0}(s)$.

**Key relationship:** The stationary distribution of $P$ is $\pi_i = \alpha_i^2 / Z$, and the eigenvalue gap of $P$ equals $\Delta(H_{S_0}(s)) / (1 - \lambda)$.

```
  ←P_{k,k-1}→   ←P_{k,k+1}→
 ·····  (k-1) ———— (k) ———— (k+1)  ·····
  0                                    L

 Random walk on L+1 sites
 Stationary distribution π_i = α_i² / Z
```

### Step 5: [[Monotonicity Bootstrap for Conductance Bounds|Monotonicity of the ground state]] (Claim 3.7)

$\alpha_0 \geq \alpha_1 \geq \cdots \geq \alpha_L \geq 0$.

**Proof idea:** Start with the all-ones vector $(1, \ldots, 1)$. Repeatedly apply $G(s)/\mu$. Since $G(s)$ preserves monotonicity of vectors (check by direct calculation), and the ground state is the limit of this iteration (by Perron's theorem), the ground state is monotone.

This is the piece that makes the conductance bound work without knowing the stationary distribution exactly. See [[Monotonicity Bootstrap for Conductance Bounds]] for the full argument.

### Step 6: Conductance bound (Claim 3.8)

For $s \geq 1/3$: $\phi(P(s)) \geq 1/(6L)$.

**Proof:** Take any subset $B$ with $\pi(B) \leq 1/2$. Find the boundary edge (the smallest $k$ where $B$ and its complement meet). The flow across this edge is $\geq \pi_{k+1}/(6)$ (using [[Monotonicity Bootstrap for Conductance Bounds|monotonicity]] + the fact that $G(s)_{k,k+1} \geq 1/6$ for $s \geq 1/3$). Since $\pi$ is monotone and supported on $L+1$ points with total weight $\geq 1/2$ on one side, $\pi_{k+1} \geq 1/(2L)$.

### Step 7: Apply the conductance bound (Theorem 2.5)

$$
\text{Eigenvalue gap of } P \geq \frac{1}{2}\phi(P)^2 \geq \frac{1}{72 L^2}
$$

Translating back: $\Delta(H_{S_0}(s)) \geq \mu / (72 L^2) \geq 1/(144 L^2)$ (using $\mu \geq 1/2$).

**Result (Lemma 3.5):** $\Delta(H_{S_0}(s)) = \Omega(1/L^2)$ for all $s$.

**5-local running time (Claim 3.9 + Lemma 3.10):** Since $S_0$ is invariant, the adiabatic evolution stays inside it and the gap $\Omega(1/L^2)$ applies directly. With $\|H_{\mathrm{final}} - H_{\mathrm{init}}\| = O(1)$ and $\|H(s)\| = O(L)$, the adiabatic time is $T = O(\epsilon^{-\delta} L^{4+2\delta})$ and the running time is $T \cdot O(L) = O(\epsilon^{-\delta} L^{5+2\delta})$. After applying [[Identity-Gate Padding for Output Amplification|identity-gate padding]] (Lemma 3.10, replacing $L$ with $2L/\epsilon$), the total running time becomes $O(\epsilon^{-(5+3\delta)} L^{5+2\delta})$.

---

## From the subspace gap to the full gap

### Legal subspace $S$

The full legal-clock subspace $S$ (dimension $(L+1) \cdot 2^n$) decomposes into $2^n$ copies $S_0, S_1, \ldots, S_{2^n - 1}$, one for each possible input $|j\rangle$. $H(s)$ is block diagonal across these subspaces.

```
 ┌─────────┐
 │  H_S₀   │
 │         │  ← input |0⟩ (the correct one)
 ├─────────┤
 │  H_S₁   │
 │         │  ← input |1⟩ (penalized by H_input)
 ├─────────┤
 │   ⋮     │
 ├─────────┤
 │ H_{S_{2ⁿ-1}} │
 └─────────┘
```

For $j \neq 0$: $H_{S_j}(s) = H_{S_0}(s) + H_{S_j,\mathrm{input}}$, where the input penalty adds eigenvalue $\geq 1$ to the $\ell = 0$ component. By **Kitaev's geometric lemma** (Kitaev-Shen-Vyalyi 2002, Lemma 14.4; stated as Lemma 3.14 in this paper):

> If $H_1, H_2$ have ground spaces at angle $\theta$ and spectral gaps $\geq \Lambda$ above their ground spaces, then the ground energy of $H_1 + H_2 \geq a_1 + a_2 + 2\Lambda \sin^2(\theta/2)$.

The angle between the ground spaces of $H_{S_0}(s)$ and $M = \mathrm{diag}(1, 0, \ldots, 0)$ satisfies $\cos\theta \leq 1 - 1/L$ (from [[Monotonicity Bootstrap for Conductance Bounds|monotonicity of the ground state]]). Combined with the $\Omega(1/L^2)$ gap of $H_{S_0}$, this gives:

$$
\text{Ground energy of } H_{S_j}(s) \geq \text{ground energy of } H_{S_0}(s) + \Omega(1/L^3)
$$

### Illegal clock states

States outside $S$ have energy $\geq 1$ from $H_{\mathrm{clock}}$. Since the ground energy of $H_S(s)$ is $\leq 1/2$ (from $\langle \gamma_0 | H(s) | \gamma_0 \rangle = s/2$), the full gap is determined by the gap within $S$.

**Result (Lemma 3.11):** $\Delta(H(s)) = \Omega(1/L^3)$.

---

## [[Identity-Gate Padding for Output Amplification|Identity-gate padding]] for output amplification (Lemma 3.10)

The [[History-State Encoding with Unary Clock|history state]] gives $|\alpha(L)\rangle$ only with probability $1/(L+1)$ upon measuring the clock. Fix: append $(2/\epsilon - 1)L$ identity gates to the circuit. Now $L' = 2L/\epsilon$ and the final state dominates the history state.

After adiabatic evolution produces a state $\epsilon/2$-close to $|\eta\rangle$, tracing out the clock gives a state $\epsilon$-close to $|\alpha(L)\rangle$ in trace distance.

This is the same idea that appears in [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|quantum ODE algorithms]] — padding the history to concentrate amplitude at the desired time. See also [[History-State Padding for Final-Time Readout]].

---

## 3-local reduction via [[Perturbation Lemma for Locality Reduction|perturbation]]

**Problem:** $H_\ell$ is 5-local (2 computation qubits + 3 clock qubits).

**Solution:** Keep the diagonal terms $|100\rangle\langle 100|$ and $|110\rangle\langle 110|$ (these are 3-local on clock qubits alone, no computation qubits needed). Replace only the off-diagonal clock terms: $|110\rangle\langle 100|_{c_{\ell-1,\ell,\ell+1}} \to |1\rangle\langle 0|_{c_\ell}$ (now 3-local: gate $U_\ell$ on 2 computation qubits + flip on 1 clock qubit).

**Problem with this:** The simplified $H'_\ell$ no longer preserves $S$ — the single-qubit clock flip $|1\rangle\langle 0|_{c_\ell}$ can create illegal clock states.

**Fix:** Crank up the penalty for illegal clocks: $J \cdot H_{\mathrm{clock}}$ with $J = \epsilon^{-2} L^6$.

**[[Perturbation Lemma for Locality Reduction|Perturbation lemma]] (Lemma 3.17):** For $H = H_1 + H_2$ where $H_2$ gives energy $\geq J$ to $S^\perp$ and $\|H_1\| \leq K$:

- Eigenvalues shift by at most $K^2/(J - 2K)$
- Ground state overlap: $|\langle \xi | \xi' \rangle|^2 \geq 1 - K^2/((b-a)(J-2K))$

With $K = O(L)$ and $J = \epsilon^{-2} L^6$, the perturbation is small enough that the gap and ground state are preserved up to $O(\epsilon)$.

**Running time:** $O(\epsilon^{-(16+4\delta)} L^{14+3\delta})$.

---

## [[Snake-Path 2D Embedding for Nearest-Neighbor Universality|2-local nearest-neighbor on a 2D grid]] (Theorem 1.3)

### The problem with going to 2D
Clock qubits and computation qubits need to interact, but on a grid each site has only 4 neighbors.

### Solution: merge clock into computation
Use **6-state particles** on an $n \times (R+1)$ grid. Each particle carries both clock and computation degrees of freedom:

| State | Phase | Meaning |
|---|---|---|
| $\bigcirc$ | Unborn | Gate hasn't reached this particle yet |
| $\uparrow\bigcirc$, $\downarrow\bigcirc$ | First phase | Active, carrying $|0\rangle$ or $|1\rangle$ |
| $\Uparrow\bigcirc$, $\Downarrow\bigcirc$ | Second phase | Active, post-gate application |
| $\times\bigcirc$ | Dead | Computation has moved past |

### The [[Snake-Path 2D Embedding for Nearest-Neighbor Universality|snake path]]

The computation propagates through the grid in a snake pattern:

```
 Round 1      Round 2      Round 3
 ↓ col 0→1   ↑ col 1→2   ↓ col 2→3

 ⇕  ○  ○     ×  ↕  ○     ×  ⇕  ○     ×  ×  ↕
 ⇕  ○  ○     ×  ↕  ○     ×  ⇕  ○     ×  ×  ↕
 ⇕  ○  ○     ×  ↕  ○     ×  ⇕  ○     ×  ×  ↕
 ↕  ○  ○     ×  ↕  ○     ×  ↕  ○     ×  ×  ↕
 ↕  ○  ○     ×  ↕  ○     ×  ↕  ○     ×  ×  ↕
 ↕  ○  ○     ↕  ○  ○     ×  ↕  ○     ×  ×  ↕

 Downward:    Upward:      Downward:
 gates go     particles    gates go
 top→bottom   move right   top→bottom
```

Each transition changes at most 2 adjacent particles → 2-local nearest-neighbor.

### Legal shapes verified locally

A shape is legal iff it contains none of the forbidden nearest-neighbor configurations (Table 1 in the paper — 10 rules checking horizontal and vertical pairs). This gives a 2-local $H''_{\mathrm{clock}}$.

### Gap analysis carries over

The restricted Hamiltonians $H''_{S,\ell}$ match the 3-local versions when projected to $S$. The same [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|Markov chain]] / conductance argument applies. The spectral gap is again $\Omega(1/L^3)$.

---

## Connections to later work

### [[Hamiltonian simulation]]
Corollary 1.2 says adiabatic computation with explicit sparse Hamiltonians is equivalent to circuit-model QC. This places sparse Hamiltonian simulation squarely in BQP. The [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders]] paper builds directly on this, providing explicit product-formula algorithms for the simulation task.

### Quantum algorithms for differential equations
The [[History-State Encoding with Unary Clock|history-state construction]] is the direct ancestor of the history-state encodings used in [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|quantum ODE/PDE algorithms]]. The [[Identity-Gate Padding for Output Amplification|identity-gate padding trick]] reappears as [[History-State Padding for Final-Time Readout|history-state padding for final-time readout]].

### QMA-completeness
The Hamiltonian constructions here (and the [[Perturbation Lemma for Locality Reduction|locality reductions]]) fed directly into Kempe-Kitaev-Regev's proof that 2-local Hamiltonian is QMA-complete.

### Spectral gap ↔ Markov chain toolkit
The paper introduces the [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|technique of mapping a quantum Hamiltonian's spectral gap problem to a classical Markov chain conductance problem]]. This became a standard tool for analyzing adiabatic algorithms.

---

## Reusable ideas

1. **[[History-State Encoding with Unary Clock]]:** Encode the trajectory of a computation in superposition using a clock register. The ground state of a local Hamiltonian captures the full computation without knowing the output.

2. **[[History-State Encoding with Unary Clock|Unary clock with local verification]]:** Represent time step $\ell$ as $|1^\ell 0^{L-\ell}\rangle$. Legality is checked by forbidding "01" substrings — a 2-local check.

3. **[[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]:** Map $G = I - H$ to a stochastic matrix $P_{ij} = (\alpha_j / \mu\alpha_i) G_{ij}$. Bound the spectral gap of $P$ via conductance (Cheeger), translate back to gap of $H$.

4. **[[Monotonicity Bootstrap for Conductance Bounds]]:** Show the ground state components are monotone by iterating $G/\mu$ on the all-ones vector. Monotonicity is all you need for the conductance bound — you don't need to know the stationary distribution.

5. **[[Identity-Gate Padding for Output Amplification]]:** Append identity gates to concentrate the [[History-State Encoding with Unary Clock|history state]] at the final time step. Trades system size for output probability.

6. **[[Perturbation Lemma for Locality Reduction]]:** Make illegal states expensive ($J \cdot H_{\mathrm{clock}}$), then bound how much the low-energy spectrum shifts. Quantitative: eigenvalue shift $\leq K^2/(J - 2K)$.

7. **[[Snake-Path 2D Embedding for Nearest-Neighbor Universality]]:** Merge clock and computation into 6-state particles on a 2D grid. The computation snakes through the lattice, changing at most 2 adjacent particles per step → nearest-neighbor 2-local.

---

## References within this paper

- Kitaev, Shen & Vyalyi (2002) — *Classical and Quantum Computation* (AMS). Source of the [[History-State Encoding with Unary Clock|history-state construction]] and the geometric lemma (Lemma 14.4)
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi, Goldstone, Gutmann & Sipser (2000)]] — initiated adiabatic QC
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov & Ta-Shma (2003)]] — adiabatic state generation, first sparse Hamiltonian simulation, showed adiabatic computation with sparse Hamiltonians can be simulated by circuits
- Reichardt (2004) — version of the adiabatic theorem used (Theorem 2.1); based on Avron & Elgart (1999)
- Kempe & Regev (2003) — 3-local Hamiltonian is QMA-complete; source of the [[Perturbation Lemma for Locality Reduction|perturbation technique]] for locality reduction
- van Dam, Mosca & Vazirani (2001) — showed adiabatic QC can be simulated by standard QC (the easy direction)
- Sinclair & Jerrum (1988) — conductance bound (Theorem 2.5) used for [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|spectral gap analysis]]

---

## Cross-links

### Paper notes
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — product-formula sparse simulation, builds on the BQP equivalence established here
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]] — uses [[History-State Encoding with Unary Clock|history-state construction]] descended from this paper
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — modern [[Order-Condition Cancellation in Product Formulas|product-formula analysis]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — optimal-scaling simulation via [[QSVT Meta-Template|QSVT]]

### Trick cards
- [[History-State Encoding with Unary Clock]]
- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]
- [[Monotonicity Bootstrap for Conductance Bounds]]
- [[Perturbation Lemma for Locality Reduction]]
- [[Identity-Gate Padding for Output Amplification]]
- [[Snake-Path 2D Embedding for Nearest-Neighbor Universality]]
- [[History-State Padding for Final-Time Readout]]
