
> **Source:** An, Costa, Berry, arXiv:2509.00171
> **Tags:** #trick #product-formulas #spectral-gap #adiabatic #discrete-adiabatic

## What it does

At large time step sizes, the spectral gap of a first-order Trotter walk operator $W(s) = e^{-if(s)H_1}e^{-i(1-f(s))H_0}$ can differ dramatically from the spectral gap of the interpolating Hamiltonian $H(s) = (1-f(s))H_0 + f(s)H_1$. The gap can close when the Hamiltonian's gap doesn't, or — more interestingly — remain open when the Hamiltonian's gap closes.

## The trick

For small $h$, the Trotter walk operator $e^{-ihf(s)H_1}e^{-ih(1-f(s))H_0}$ has eigenvalues close to those of $e^{-ihH(s)}$, with perturbation bounded by the [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|second-order commutator norm]]:

$$|gap(W) - h \cdot gap(H)| \leq \frac{h^3}{95}(2\|[H_1,[H_1,H_0]]\| + \|[H_0,[H_0,H_1]]\|)$$

But at $h = O(1)$ (e.g. $h = 1$), this perturbative relation breaks down completely. The eigenvalues of $W(s)$ live on the unit circle and can rearrange in ways with no analogue in the Hamiltonian spectrum.

**Case 1 (gap opens):** $H(s)$ is gapless — the two lowest eigenvalues cross. But $W(s) = e^{-if(s)H_1}e^{-i(1-f(s))H_0}$ with $h = 1$ can maintain a finite gap. This means discrete adiabatic evolution succeeds where continuous AQC fails. Reducing $h$ actually makes things worse by forcing the numerical evolution to track the gapless continuous dynamics.

**Case 2 (gap closes):** $H(s)$ has a constant gap, but the walk operator's gap closes at the same interpolation point. This is the dangerous direction — you can't assume the Trotter operator inherits the Hamiltonian's gap at large step sizes.

Both cases are demonstrated on 4-dimensional toy models in the paper. The critical point: for the $2 \times 2$ Grover search subspace, the walk operator gap is always $\geq \frac{2}{3}$ times the Hamiltonian gap (Lemma 14), which is why the Grover application works cleanly.

## When to reach for it

- Considering first-order Trotter for AQC on problems where the Hamiltonian gap is exponentially small or zero.
- Understanding why large-step-size Trotter sometimes works better than expected (and sometimes worse).
- Designing discrete adiabatic algorithms that exploit gap restructuring under Trotterisation.

## Complexity

If the walk operator has gap $\Delta_W$ while the Hamiltonian is gapless, discrete adiabatic evolution can prepare the eigenstate in $O(1/\Delta_W)$ steps (per the [[Discrete Adiabatic Theorem for Quantum Walks|discrete adiabatic theorem]]) — which is potentially exponentially cheaper than continuous AQC, which requires $\Omega(1/\Delta_H)$ time.

## Caveat

There's no general theory for predicting when the Trotter walk operator's gap will open or close relative to the Hamiltonian's. The toy models are 4-dimensional and hand-crafted. The paper poses finding practical applications with this "gap opening" property as an open question. For generic many-body systems, it's unclear whether this phenomenon is common or exotic.

## Related notes
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]]
- [[Discrete Adiabatic Theorem for Quantum Walks]]
- [[Discrete Adiabatic View of Numerical Integrators]]
- [[Error-Independent Time Step for Adiabatic Simulation]]
- [[Cross-Order Product Formula Threshold]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
