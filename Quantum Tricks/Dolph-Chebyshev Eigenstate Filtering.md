
> **Source:** Costa, An, Sanders, Su, Babbush, Berry, arXiv:2111.08152
> **Tags:** #trick #eigenstate-filtering #LCU #Chebyshev #linear-systems #qubitization

## What it does

Filters a quantum state to suppress components outside the spectrum of interest, using a linear combination of powers of a walk operator with Dolph-Chebyshev window weights. Achieves $O(\kappa \log(1/\varepsilon))$ cost for the QLSP — same as QSP/singular-value-processing-based filtering, but with **closed-form rotation angles** and only **two ancilla qubits**.

## The trick

Given a [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk $W$ with eigenvalues $e^{i\phi_k}$, construct a filter function via LCU:

$$\tilde{w}(\phi) = \frac{1}{\sum_j w_j} \sum_j w_j e^{ij\phi}$$

Choose the weights $w_j$ as the discrete Fourier transform of a Dolph-Chebyshev window:

$$\tilde{w}(\phi) = \varepsilon \, T_\ell(\beta \cos \phi)$$

where $T_\ell$ is the degree-$\ell$ Chebyshev polynomial and $\beta = \cosh(\cosh^{-1}(1/\varepsilon)/\ell)$.

The filter peaks at $\phi = 0, \pi$ (the eigenvalues corresponding to the zero-energy eigenspace of interest) and suppresses everything beyond a width set by matching $\beta \cos(1/\kappa) = 1$. Solving for $\ell$:

$$\ell = \frac{\cosh^{-1}(1/\varepsilon)}{\cosh^{-1}(1/\cos(1/\kappa))} \leq \kappa \ln(2/\varepsilon)$$

**Implementation trick:** Encode the control register in **unary** (one qubit per controlled walk step). Prepare each qubit by a controlled rotation from the previous one, apply the controlled-$W$, then immediately **measure** (no need to hold all $\ell$ qubits simultaneously). The inverse preparation runs in the opposite direction, so you only need **two ancilla qubits** cycling in alternation (Figures 6–7 in the paper).

Positive and negative powers of $e^{i\phi}$ are obtained simultaneously by controlling whether the reflection step in the qubitized walk is applied — this costs essentially nothing extra.

**Early failure detection:** Because measurements happen sequentially, a failure (wrong outcome) is flagged on average halfway through the circuit. You can discard and restart, saving ~50% of the expected cost on failed runs.

## When to reach for it

- Post-processing an approximate ground state from adiabatic preparation or QPE.
- Any QLSP algorithm that achieves constant overlap and needs to boost to precision $\varepsilon$.
- When you want QSP-equivalent filtering but don't want to solve the QSP angle-finding problem.
- Early fault-tolerant settings where implementation simplicity matters as much as asymptotic cost.

## Complexity

$\ell \leq \kappa \ln(2/\varepsilon)$ applications of the walk operator $W$. This is half the cost of singular value processing (which uses polynomial order $2\ell$), though the original SVP analysis in Lin-Tong overestimates their cost by $\sqrt{2}$ — the actual costs are identical.

Two ancilla qubits for the control register (vs. one for SVP). $O(\ell)$ single-qubit rotations with closed-form angles.

## Caveat

Requires the initial state to have $\geq 1/2$ overlap with the spectrum of interest. If the overlap is worse, you need more repetitions (expected cost scales as $1/p_{\text{success}}$).

The two-qubit-recycling trick requires adaptive circuits (measurement + classical feed-forward). On architectures where mid-circuit measurement is expensive, the standard $\lceil\log_2 \ell\rceil$-qubit binary encoding may be preferable.

## Related notes
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — Analyzes the combined cost of adiabatic + filtering stages with optimized intermediate error
- [[Adiabatic–Filter Cost Partitioning]] — The cost optimization between adiabatic and filtering steps
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Coherent Alias Sampling for PREPARE]]
