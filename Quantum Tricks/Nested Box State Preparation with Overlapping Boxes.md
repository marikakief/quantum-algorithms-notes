# Nested Box State Preparation with Overlapping Boxes

> **Source:** Su, Berry, Wiebe, Rubin, Babbush, arXiv:2105.12767, PRX Quantum 2, 040332 (2021) [non-overlapping]; Berry, Rubin, Babbush et al., arXiv:2312.07654 (2024) [overlapping extension]
> **Tags:** #trick #state-preparation #block-encoding #LCU #first-quantized #amplitude-amplification

---

## The problem this solves

In first-quantized plane-wave quantum chemistry, the Coulomb potential is a sum over momentum transfers $\nu$:

$$U + V \propto \sum_{\nu \neq 0} \frac{1}{\|k_\nu\|^2} \cdot (\text{something acting on electron registers})$$

To block-encode this as an LCU, the PREPARE oracle needs to create the weighted superposition

$$|\text{PREP}\rangle = \frac{1}{\sqrt{\lambda}} \sum_{\nu} \sqrt{w(\nu)} \, |\nu\rangle$$

where $w(\nu) \propto 1/\|k_\nu\|$ or $1/\|k_\nu\|^2$ depending on the formulation. The weights vary smoothly over a 3D grid of $O(N)$ momentum modes.

**Why not just load the weights directly?** You could store all $N$ coefficients in a QROM table and use [[Coherent Alias Sampling for PREPARE|alias sampling]], but $N$ can be $10^5$–$10^8$ for realistic simulations. That's a lot of classical data to load. The nested box construction exploits the *structure* of the weight function — it's roughly $1/\|k_\nu\|^2$, which depends only on distance from the origin — to prepare the superposition with $O(\log N)$ cost, no classical data tables needed.

---

## The idea: dyadic shells in momentum space

Partition momentum space into regions where $1/\|k_\nu\|^2$ is roughly constant. The natural choice: concentric shells at dyadic (powers-of-2) distances from the origin. Within each shell, the weight varies by at most a factor of ~4, so a uniform superposition within the shell, rescaled by the shell's characteristic weight, is a decent approximation.

Concretely, define **boxes** $B_\mu$ for $\mu = 0, 1, \ldots, n_p - 1$ as rectangular regions:

$$B_\mu = \{\nu \in \mathbb{Z}^3 : |\nu_d| < 2^{\mu} \text{ for } d \in \{x,y,z\}\}$$

Each $B_\mu$ contains $O(2^{3\mu})$ lattice points — roughly $8\times$ more than the previous box.

---

## Version 1: Non-overlapping shells (Su et al. 2021)

Define the **shell** $S_\mu = B_\mu \setminus B_{\mu-1}$ — the points in box $\mu$ but not in any smaller box. These shells partition momentum space into disjoint regions.

**Algorithm:**

1. **Prepare $\mu$:** Create a superposition $\sum_\mu \psi_\mu |\mu\rangle$ where $\psi_\mu \propto 2^{-\mu}$ (reflecting that $1/\|k_\nu\| \sim 2^{-\mu}$ in shell $\mu$, and there are $\sim 2^{3\mu}$ points). This uses $O(n_p)$ controlled Hadamards.

2. **Prepare $\nu$ within shell:** Create a uniform superposition over $\nu \in B_\mu$ (easy — just Hadamards on $\mu$ qubits per dimension), then **test** whether $\nu \in S_\mu$ (i.e., $\nu \notin B_{\mu-1}$). If the test fails, the preparation fails.

3. **Inequality test for amplitude:** Compare a uniform ancilla register against $M \cdot (2^{\mu-2}/\|k_\nu\|)^2$ where $M$ is a precision parameter. This [[Inequality-Test Amplitude Transduction|transduces the classical value into a quantum amplitude]] proportional to $1/\|k_\nu\|$.

4. **If both tests pass:** the state has the correct approximate weighting. **If either fails:** flag as failure.

**Success probability:** The shell membership test fails about 1/8 of the time (the inner box $B_{\mu-1}$ is 1/8 the volume of $B_\mu$). Combined with the inequality test, overall success probability is $\sim 1/4$.

**Cost of failure:** With probability $\sim 3/4$ you need to retry. One round of amplitude amplification (3 applications of the PREPARE circuit and its inverse) boosts success to $\sim 1$. This triples the cost of the most expensive step — computing $\|k_\nu\|^2$ for the inequality test, which costs $O(n_p^2)$ Toffolis.

---

## Version 2: Overlapping boxes (Berry et al. 2024)

The key observation: **the shell membership test is wasteful**. You're throwing away valid $\nu$ vectors just because they also belong to a smaller box. What if you let them contribute from multiple boxes?

Instead of disjoint shells $S_\mu = B_\mu \setminus B_{\mu-1}$, use the **full boxes** $B_\mu \subset B_{\mu+1} \subset \cdots$, with overlaps.

**Algorithm:**

1. **Prepare $\mu$:** Create $\sum_\mu \tilde{\psi}_\mu |\mu\rangle$ (with modified weights — see below).

2. **Prepare $\nu$ within $B_\mu$:** Uniform superposition over $\nu \in B_\mu$. No shell test needed — every $\nu \in B_\mu$ is kept.

3. **The state is now:**
$$\sum_\mu \tilde{\psi}_\mu \frac{1}{\sqrt{|B_\mu|}} \sum_{\nu \in B_\mu} |\mu\rangle |\nu\rangle$$

4. **After tracing out (or uncomputing) $\mu$**, each $\nu$ has total squared amplitude:
$$\text{amp}^2(\nu) = \sum_{\mu : \nu \in B_\mu} \frac{\tilde{\psi}_\mu^2}{|B_\mu|}$$

Since $\nu \in B_\mu$ for all $\mu \geq \mu_{\min}(\nu)$ (the smallest box containing $\nu$), this is a *sum* over all boxes that contain $\nu$. Points near the origin (small $\mu_{\min}$) get contributions from many boxes; points far out get contributions from few.

**Choosing the weights $\tilde{\psi}_\mu$:** The weights are chosen so that $\text{amp}^2(\nu)$ upper-bounds $w(\nu)$ for all $\nu$. Specifically:

$$\tilde{\psi}_\mu^2 / |B_\mu| = \max_{\nu \in B_{\mu_{\max}} \setminus B_{\mu-1}} w(\nu) - \max_{\nu \in B_{\mu_{\max}} \setminus B_\mu} w(\nu)$$

This is a telescoping construction: the contribution from box $\mu$ equals the *incremental* maximum of $w$ gained by including shell $\mu$. The sum then telescopes to $\max_{\nu \in B_{\mu_{\max}} \setminus B_{\mu_{\min}-1}} w(\nu) \geq w(\nu)$ for all $\nu$ in the summation range. Crucially, this is non-negative (each term $\geq 0$) because the maximum is taken over a *shrinking* set.

**Why this helps:**
- **No shell membership test** → no failures from that step
- **Each $\nu$ counted multiple times** → higher effective success probability for the inequality test
- **Can skip amplitude amplification entirely** if you accept a modest increase in $\lambda$ (the LCU 1-norm)

The tradeoff: the box-level upper bound inflates $\lambda$ compared to the exact weighting. For $w(\nu) = 1/\|k_\nu\|^2$, the inflation is modest — $w$ varies by at most $\sim 4\times$ within each box. For nonlocal pseudopotentials, the inflation can be $\sim 10\times$.

---

## Generalisation to non-cubic cells

For non-cubic simulation cells (different numbers of plane waves in each direction), the boxes become rectangular with direction-dependent offsets:

$$B_\mu = \{\nu : |\nu_d| < 2^{\mu - \delta_d - 1} \text{ for } d \in \{x,y,z\}\}$$

The offsets $\delta_x, \delta_y, \delta_z \geq 0$ are tuned to the aspect ratio of the cell and optimised numerically for each crystal structure.

---

## When to reach for it

- Any block encoding where the PREPARE weights are a smooth function of a register value (not discrete table entries)
- The function should have a roughly dyadic decay structure — big near the origin, falling off with distance
- You have $O(N)$ grid points and can't afford to store all coefficients
- Particularly suited to Coulomb-like $1/r^2$ or $1/r$ potentials in momentum space

---

## Complexity

| Version | Per-step cost | Amplitude amplification | Net cost |
|---|---|---|---|
| Non-overlapping (Su et al.) | $O(n_p^2 + n_M n_p)$ Toffolis | 1 round ($3\times$ the $\|k_\nu\|^2$ computation) | $30n_p + 2b$ Toffolis |
| Overlapping (Berry et al.) | Same base cost | None needed | $9n_p + 2b$ Toffolis |

Here $n_p = \lceil\log(N^{1/3}+1)\rceil$ is the bits per momentum register dimension and $b$ is the precision parameter for the inequality test.

---

## Caveat

- The overlapping version inflates $\lambda$ by a structure-dependent factor. For Coulomb $1/\|k_\nu\|^2$: factor of ~2–4. For pseudopotentials: up to ~10×. Since the total algorithm cost scales as $O(\lambda/\varepsilon)$, this inflation partially offsets the savings from skipping amplitude amplification.
- Both versions assume the weight function is well-approximated by its maximum within each box. For oscillatory or non-monotone weights, the box-level upper bound would be very loose.
- The box hierarchy only helps for weights that decay with distance. For flat or increasing weights, you'd need a different construction.

---

## Related notes

- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — original non-overlapping construction
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — overlapping extension
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — uses the box construction for stopping power
- [[Inequality-Test Amplitude Transduction]] — the comparator trick used inside each box to convert classical values to amplitudes
- [[Failed PREP Fallback to Alternative Hamiltonian Term]] — complementary trick that also avoids amplitude amplification (for the Coulomb term specifically)
- [[Coherent Alias Sampling for PREPARE]] — the alternative approach when you have discrete coefficients and can afford the QROM
- [[First-Quantized Plane-Wave Chemistry Encoding]] — the broader context for why you're working in momentum space
