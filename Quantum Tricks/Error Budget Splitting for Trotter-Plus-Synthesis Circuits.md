# Error Budget Splitting for Trotter-Plus-Synthesis Circuits

## What it does

When implementing a product-formula circuit on a fault-tolerant machine, two independent sources of error must both be controlled within a total budget $\epsilon$: (1) the Trotter formula approximation error $\epsilon_T$, and (2) the per-gate rotation synthesis error $\epsilon_S$ accumulated over all gates. Naively setting both to $\epsilon/2$ is suboptimal. This trick says how to split the budget to minimize total T-gate cost.

## The trick

Let $r$ be the number of Trotter steps, $k$ the formula order, and $G$ the number of single-qubit rotations per step. The number of T gates scales as

$$T_{\text{total}} \approx r \cdot G \cdot c \cdot \log(1/\epsilon_S)$$

where $c$ is a small constant (e.g., $c \approx 3$ for gridsynth), while the Trotter error constraint gives

$$r \geq \left(\frac{C \Lambda^{1+1/2k} t^{1+1/2k}}{\epsilon_T^{1/2k}}\right)^{2k/(2k+1)}$$

Setting $\epsilon_S = \epsilon / (r \cdot G)$ (one part of the total error per gate) and $\epsilon_T = \epsilon - r G \epsilon_S$, then jointly minimizing $T_{\text{total}}$ over $r$, $k$, and the split $\epsilon_T + \epsilon_S \leq \epsilon$ gives the optimal allocation.

The key insight from [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes|Childs-Maslov-Nam-Ross-Su 2018]]: for the FeMo cofactor, the optimal choice allocates *most* of the error budget to Trotter error and only a small fraction to synthesis error, because $\log(1/\epsilon_S)$ grows slowly while cutting $\epsilon_T$ forces $r$ to grow superpolynomially.

Formally, optimal splitting satisfies approximately

$$\epsilon_T \approx \epsilon \cdot \frac{2k}{2k+1}, \quad \epsilon_S \approx \epsilon \cdot \frac{1}{2k+1} \cdot \frac{1}{rG}$$

## When to reach for it

- Compiling any product-formula Hamiltonian simulation circuit to Clifford+T gates.
- Any algorithm where approximation layers have independent additive errors that accumulate.
- Resource estimation: computing concrete T-gate counts for fault-tolerant chemistry simulations.

## Complexity

Applying the optimal split to a $2k$th-order formula gives total T-gate count approximately

$$T_{\text{total}} = O\!\left( G \cdot C^{2k/(2k+1)} (\Lambda t)^{(2k+1)/(2k)} \epsilon^{-1/(2k)} \cdot \log\!\left(\frac{G \Lambda t}{\epsilon}\right) \right)$$

For $k=6$ (12th-order), the dependence on $1/\epsilon$ is $\epsilon^{-1/12}$, nearly negligible compared to lower orders.

## Caveat

- The synthesis cost formula assumes gridsynth or equivalent optimal Clifford+T single-qubit rotation synthesis. Solovay-Kitaev costs scale as $\log^{3.97}(1/\epsilon_S)$ and would shift the optimum.
- If gates can be parallelized on the fault-tolerant hardware, the relevant metric is T-depth rather than T-count, and the split should account for circuit depth.
- The formula assumes all rotation angles are generic (irrational multiples of $\pi$). If many angles are special values (multiples of $\pi/8$), some rotations are exact Clifford+T and need no synthesis.

## Related notes

- [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes]] — source paper
- [[Suzuki Order as a Tunable Knob for Time Scaling]] — companion trick for choosing $k$
- [[Pauli Ladder Reuse for T-Gate Reduction in Trotter Steps]] — reducing $G$ before applying the split
- [[Error Budget Optimization for Fault-Tolerant Algorithms]] — general error budget framework
- [[Dirty Qubit Recycling for T-Gate Reduction]] — orthogonal technique for T-gate savings
- [[Product Formulas]] — concept hub
