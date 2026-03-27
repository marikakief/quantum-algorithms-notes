
> **Source:** Su, Berry, Wiebe, Rubin, Babbush, arXiv:2105.12767 (2021)
> **Tags:** #trick #kinetic-energy #interaction-picture #incremental-update #arithmetic

## What it does

Reduces the cost of exponentiating the kinetic operator $e^{-iT\tau}$ in the interaction picture from $O(\eta n_p^2)$ Toffolis per step to $O(n_p^2 + n_\eta)$, by maintaining a running accumulator for $\sum_j \|k_{p_j}\|^2$ and updating it incrementally when momentum registers change.

## The trick

In the [[Interaction Picture Simulation (Kinetic Frame)|interaction picture]] simulation of first-quantized chemistry, the Dyson series interleaves applications of $e^{-iT\tau}$ with block encodings of $U + V$. Each $e^{-iT\tau}$ requires the total kinetic energy $E_T = \sum_{j=1}^\eta \|k_{p_j}\|^2/2$ to compute the phase.

Naively, this means squaring $3\eta$ momentum components ($3\eta n_p^2$ Toffolis) at each of $K+1$ kinetic-energy phasing steps. With $\eta = 100$ and $n_p \approx 7$, that's $\sim 15{,}000$ Toffolis per step.

Instead: compute $E_T$ once at the start of the algorithm and store it in an ancilla register. When the potential block encoding shifts momentum $p_i \to p_i + \nu$ and $p_j \to p_j - \nu$, update the register:

$$\|p_i + \nu\|^2 - \|p_i\|^2 = 2p_i \cdot \nu + \|\nu\|^2$$
$$\|p_j - \nu\|^2 - \|p_j\|^2 = -2p_j \cdot \nu + \|\nu\|^2$$

The value $\|\nu\|^2$ is already computed during the PREPARE for the $1/\|\nu\|$ weights. The dot products $p \cdot \nu$ cost $2n_p^2 - 3n_p$ Toffolis each (multiply absolute values of each component, sign from CNOT). Adding to the accumulator: $O(n_\eta + n_p)$ per component. Total update per potential step:

$$12n_p^2 + 2n_p + 8n_\eta$$

— **independent of $\eta$** up to the $n_\eta = \lceil\log\eta\rceil$ term. For $\eta = 100$, $n_p = 7$: the update costs $\sim 650$ Toffolis vs. $\sim 15{,}000$ for recomputation. A $23\times$ savings.

The energy offset $E_0$ (for placing the estimated eigenvalue near zero) is subtracted from the register at the start, giving the phase shift $e^{-i(E_T - E_0)\tau}$ at every step for free.

## When to reach for it

- Interaction picture simulation where $A$ is a function of registers that $B$ modifies
- Any simulation framework where the same global quantity (e.g., total energy, total momentum) must be evaluated repeatedly, and each step modifies only $O(1)$ registers
- Most valuable when $\eta$ is large (many particles), so recomputation from scratch is expensive

## Complexity

Per potential block encoding step: $12n_p^2 + 2n_p + 8n_\eta$ Toffolis for the update.

Per kinetic energy phasing: $2n_t(n_\eta + 2n_p) - n_t + b_{\mathrm{grad}} - 2$ Toffolis (multiplication of the accumulator by the time register and addition into the phase gradient state).

Compare to full recomputation: $3\eta n_p^2$ Toffolis per step. Savings factor: $\sim 3\eta n_p^2 / (12n_p^2 + 8n_\eta) \approx \eta/4$ for typical $n_p$.

## Caveat

The accumulator register must be wide enough to avoid overflow during intermediate steps (width $n_\eta + 2n_p$ bits). The phase is applied via a modified phase gradient state, and the time $\tau$ must be adjusted so that $2\tau\pi^2/\Omega^{2/3}$ is an integer multiple of $2\pi/2^{b_{\mathrm{grad}}}$ — this is handled by a small ($< 10^{-6}$ relative) adjustment to $\tau$.

Also requires maintaining the accumulator correctly through all the controlled swaps of momentum registers, which adds a small constant overhead (4 Toffolis for controlling the $\|\nu\|^2$ additions).

## Related notes
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
- [[Interaction Picture Simulation (Kinetic Frame)]] — the simulation framework where this is applied
- [[First-Quantized Plane-Wave Chemistry Encoding]] — the representation that makes $T$ diagonal
- [[Kinetic Energy as Bit-Product LCU]] — the complementary trick for qubitization (eliminates kinetic arithmetic entirely)
