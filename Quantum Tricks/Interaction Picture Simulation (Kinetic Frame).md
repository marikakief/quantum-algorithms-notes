# Interaction Picture Simulation (Kinetic Frame)

> **Source:** Babbush, Berry, McClean, Neven, arXiv:1807.09802 (2019); technique from Low & Wiebe, Quantum (2019), arXiv:1805.00675
> **Tags:** #trick #interaction-picture #LCU #Hamiltonian-simulation #first-quantized #sublinear #Dyson-series

## What it does

Simulates $e^{-i(A+B)t}$ with cost scaling in $\lambda_B$ (the LCU 1-norm of the smaller piece $B$) rather than $\|A+B\|$, by working in the rotating frame of the large piece $A$. For first-quantized plane-wave chemistry, choosing $A = T$ (kinetic) and $B = U+V$ (potential) gives gate complexity $\widetilde{O}(\lambda_B t) = \widetilde{O}(\eta^{5/3} N^{1/3} t)$ per oracle call, yielding total cost $\widetilde{O}(\eta^{8/3} N^{1/3} t)$.

## The trick

For Hamiltonians $H = A + B$ where the two pieces have very different norms ($\|A\| \gg \|B\|$, or more precisely $\|A\| \gg \lambda_B$), standard LCU simulation pays a cost proportional to $\lambda_B + \|A\|$ per time step. The interaction picture avoids the large $\|A\|$ contribution entirely.

**The Dyson expansion in the interaction picture:**

$$e^{-i(A+B)t} \approx \sum_{k=0}^{K-1} (-i)^k \int_0^t dt_1 \int_{t_1}^t dt_2 \cdots \int_{t_{k-1}}^t dt_k\, \mathcal{I}_k$$

$$\mathcal{I}_k = e^{-iA(t-t_k)}\, B\, e^{-iA(t_k-t_{k-1})}\, B \cdots e^{-iA(t_2-t_1)}\, B\, e^{-iAt_1}$$

This time-ordered product interleaves exact evolutions under $A$ with applications of $B$ at discrete times. The integral is approximated by discretizing time into segments and implementing the resulting expression as an LCU (via oblivious amplitude amplification).

**Complexity (Low & Wiebe Lemma 6):**

- Number of time segments: $O(\lambda_B t)$
- Truncation order per segment: $K = O(\log(\lambda_B t/\varepsilon) / \log\log(\lambda_B t/\varepsilon))$
- Total calls to $e^{-iA\tau}$ and LCU oracles for $B$:

$$O\!\left(\lambda_B t \cdot \frac{\log(\lambda_B t/\varepsilon)}{\log\log(\lambda_B t/\varepsilon)}\right)$$

- Additional $\log(t\|A\|/\varepsilon\lambda_B)$ multiplicative factor for gate complexity (from ancilla state preparation for the time discretization)

The critical point: **$\|A\|$ appears only logarithmically**, not polynomially. This makes the technique extremely valuable when $\|A\|$ is large but $\lambda_B$ is small.

**Application to first-quantized chemistry (Babbush et al. 2019):**

In first-quantized momentum space with $\Omega \propto \eta$:

$$\|T\| = O\!\left(\frac{N^{2/3}}{\eta^{1/3}}\right) \quad \text{and} \quad \lambda_{U+V} = O\!\left(\eta^{5/3} N^{1/3}\right)$$

So $\|T\|/\lambda_{U+V} = O(N^{1/3}/\eta^2)$ — kinetic energy dominates by a polynomial factor when $N \gg \eta^6$, and both terms are polynomial in $N$ otherwise. Choosing $A = T$, $B = U+V$:

- Total gate cost: $\widetilde{O}(\lambda_B t \cdot (\eta \log^2 N)) = \widetilde{O}(\eta^{8/3} N^{1/3} t)$
- The $\|T\|$ contribution appears only as a $\log$ factor

**Contrast with Low & Wiebe (2018):** That paper applied the interaction picture to *second-quantized* plane-wave chemistry, choosing $A = U+V$ (potential, large in second quantization) and $B = T$ (kinetic, small relative to potential in second quantization). The physics is reversed in first quantization, which is why this paper gets a better scaling.

**Implementation of $e^{-iA\tau} = e^{-iT\tau}$:**

In momentum space, $T$ is diagonal: $T|p\rangle_j = \|k_p\|^2/2 \cdot |p\rangle_j$ for each electron $j$. So:

$$e^{-iT\tau} = \sum_{p_\ell \in G} \exp\!\left[-\frac{i\tau}{2}\sum_{j=1}^\eta \|k_{p_j}\|^2\right] |p_1\rangle\langle p_1| \otimes \cdots \otimes |p_\eta\rangle\langle p_\eta|$$

Compute $\sum_j \|k_{p_j}\|^2$ into an ancilla (cost $O(\eta \log^2 N)$ with elementary multiplication), apply a phase rotation, uncompute. This is one of the cheapest possible $A$-evolution implementations.

## When to reach for it

- $H = A + B$ where the norms (or LCU 1-norms) of $A$ and $B$ differ significantly
- $A$ is easy to simulate exactly and cheaply (e.g., diagonal in the computational basis)
- $B$ has a small LCU 1-norm $\lambda_B$ even if it has many terms
- The goal is time evolution or phase estimation (not just state preparation)

More specifically: reach for this when standard LCU would give cost $O((\lambda_A + \lambda_B)t)$ but $\lambda_A \gg \lambda_B$, and $e^{-iAt}$ can be implemented at much lower cost than $\lambda_A$ would suggest.

## Complexity

For $H = A + B$, number of queries to $e^{-iA\tau}$ and $B$-oracle:

$$\widetilde{O}\!\left(\lambda_B t\right)$$

(suppressing $\log$ factors in $\lambda_B t/\varepsilon$ and $t\|A\|/\varepsilon\lambda_B$).

Total gate complexity adds per-query costs of implementing $e^{-iA\tau}$ and the SELECT/PREPARE oracles for $B$.

For first-quantized plane-wave chemistry specifically:

$$\text{Total gate complexity} = \widetilde{O}\!\left(\eta^{8/3} N^{1/3} t\right)$$

## Caveat

- Requires that $e^{-iA\tau}$ can be implemented efficiently. If $A$ is not diagonal (or otherwise easy to evolve exactly), the method loses most of its advantage.
- The extra $\log(t\|A\|/\varepsilon\lambda_B)$ factor in gate complexity is benign when $\|A\|/\lambda_B$ is polynomial, but can grow if $\|A\|$ is exponentially large relative to $\lambda_B$.
- The Dyson expansion requires time discretization; errors from the discretization and from truncating the expansion at order $K$ must both be bounded. The Low & Wiebe analysis handles this, but it requires careful implementation of the LCU control register for time variables.
- Does not directly reduce *query* complexity for phase estimation (which is Heisenberg-limited at $O(\lambda/\varepsilon)$ for [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]); the benefit is in reducing $\lambda$ by choosing the right splitting.

## Related notes
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — compiles the interaction picture to explicit circuits; introduces [[Incremental Kinetic Energy Register]] and [[Qubitized Dyson Series for Phase Estimation]] to reduce constant factors
- [[Truncated Taylor Series Simulation]] — the Taylor series / LCU method the interaction picture builds on
- [[First-Quantized Plane-Wave Chemistry Encoding]] — the encoding that makes $A = T$ cheap to implement
- [[Incremental Kinetic Energy Register]] — reduces per-step kinetic cost from $O(\eta n_p^2)$ to $O(n_p^2)$
- [[Qubitized Dyson Series for Phase Estimation]] — replaces amplitude amplification with qubitization of $\sin(H\tau)$
- [[Plane-Wave Dual Basis]] — Low & Wiebe's complementary choice ($A = U+V$ in second quantization)
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — alternative framework; achieves Heisenberg-limited query complexity without needing the interaction picture split
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — uses the same general LCU/Taylor series technique in second quantization
