# Nested Box State Preparation with Overlapping Boxes

> **Source:** Berry, Rubin, Babbush et al., arXiv:2312.07654; extends Su, Berry et al., arXiv:2105.12767
> **Tags:** #trick #state-preparation #block-encoding #LCU #amplitude-amplification #first-quantized

## What it does

Prepares a superposition over momentum transfers $\nu$ with approximately correct amplitude weighting, using a hierarchy of overlapping rectangular boxes $B_\mu$ in 3D momentum space. The overlap between boxes boosts the success probability of the state preparation (avoiding expensive amplitude amplification) while maintaining correctness of the block encoding.

## The trick

In [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] first-quantized plane-wave simulation, the PREPARE oracle must create a superposition:

$$\sum_{\nu} \sqrt{u(\nu)} |\nu\rangle$$

where $u(\nu)$ is a function like $1/\|k_\nu\|^2$ (for the Coulomb interaction) or a pseudopotential function.

**Prior approach (Su et al. 2021):** Use *non-overlapping* nested boxes $B_\mu \setminus B_{\mu-1}$, each a shell at distance scale $\sim 2^\mu$. Prepare a superposition over $\mu$, then a uniform superposition over $\nu \in B_\mu \setminus B_{\mu-1}$, then inequality test. Success probability $\sim 1/4$, requiring one round of amplitude amplification ($3\times$ the cost of computing $\|k_\nu\|^2$).

**This paper's approach:** Use *overlapping* boxes where $B_\mu \subset B_{\mu+1}$, so a given $\nu$ appears in multiple boxes. The prepared state is:

$$\sum_\mu \psi_\mu \frac{1}{\sqrt{|B_\mu|}} \sum_{\nu \in B_\mu} |\mu\rangle |\nu\rangle$$

After tracing out $\mu$, each $\nu$ has total (squared) amplitude $\sum_{\mu' \geq \mu(\nu)} \psi^2_{\mu'} / |B'_{\mu'}|$, where $\mu(\nu)$ is the smallest $\mu$ such that $\nu \in B_\mu$. The weights $\tilde{\psi}_\mu$ are chosen so this sum equals $\max_{\nu' \in B_{\mu_{\max}} \setminus B_{\mu(\nu)-1}} u(\nu')$ — an upper bound using the maximum value over the complementary region.

Key properties:
- **Higher success probability** than non-overlapping boxes (because each $\nu$ is counted multiple times)
- **Monotonicity is guaranteed**: $\tilde{\psi}^2_\mu \geq 0$ because the maximum is taken over $B_{\mu_{\max}} \setminus B_{\mu-1}$, which gets *smaller* as $\mu$ increases
- **For the pseudopotential**: this approach is used *without* amplitude amplification — the approximate amplitude from the boxes is combined with an inequality test in SELECT to apply the exact amplitude factor. The box-level upper bound increases $\lambda$ but eliminates the $3\times$ cost of amplitude amplification.

The boxes are generalised for non-cubic cells: $B_\mu = \{\nu : |\nu_x| < 2^{\mu-\delta_x-1}, |\nu_y| < 2^{\mu-\delta_y-1}, |\nu_z| < 2^{\mu-\delta_z-1}\}$ with offsets $\delta_x, \delta_y, \delta_z \geq 0$ to handle different numbers of bits in each direction. The offsets are optimised per crystal structure.

## When to reach for it

- Any block encoding where the PREPARE weights depend on a continuous function of a register value (not just discrete table entries)
- When amplitude amplification is too expensive and you can tolerate a modest increase in $\lambda$
- Particularly useful when the function $u(\nu)$ varies smoothly within each box — the box-level upper bound is then a good approximation

## Complexity

Preparing the superposition over $\mu$ and $\nu$ within $B_\mu$: $O(n)$ Toffolis per iteration (controlled Hadamards, inner-box testing, negative-zero elimination), where $n = \max(n_x, n_y, n_z)$. With amplitude amplification for $1/\|k_\nu\|$: $30n + 2b$ Toffolis (tripled for amplitude amplification). Without amplitude amplification (pseudopotential): just $9n + 2b$.

The offsets $\delta_x, \delta_y, \delta_z$ are tuned so that with amplitude amplification, the success probability is $\gtrsim 1/4$ (close to the maximum achievable with one round). Table XV in the paper lists optimal offsets for various crystal structures.

## Caveat

Using upper bounds within boxes inflates $\lambda$ by a factor that depends on how quickly $u(\nu)$ varies within each box. For the Coulomb $1/\|k_\nu\|^2$: the inflation is modest (the function varies by at most a factor of $\sim 4$ across a box). For the nonlocal pseudopotential: the inflation is more significant — $\lambda_{\text{nonloc}}$ from Eq. (80) in the paper (with box-level maximisation) can be $\sim 10\times$ larger than the tight value from Eq. (79).

## Related notes
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — The paper extending this technique
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Original non-overlapping nested box approach
- [[Failed PREP Fallback to Alternative Hamiltonian Term]] — Complementary trick that avoids amplitude amplification for the Coulomb term
- [[Coherent Alias Sampling for PREPARE]] — Alternative state preparation for discrete coefficient sets
