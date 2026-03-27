
> **Source:** Wiebe, Braun, Lloyd, arXiv:1204.5242 (2012); implicit in Harrow, Hassidim, Lloyd, arXiv:0811.3171 (2009)
> **Tags:** #trick #phase-estimation #eigenvalue #HHL #linear-algebra #quantum-simulation

## What it does
Implements multiplication by eigenvalues of a Hermitian operator — as opposed to HHL's eigenvalue inversion — using the same phase-estimation circuit. Both operations have the same cost.

## The trick

In HHL, the circuit is:
1. Prepare clock register $|\Psi_0\rangle = \sqrt{2/T}\sum_\tau \sin(\pi(\tau+\tfrac12)/T) |\tau\rangle \otimes |\psi\rangle$.
2. Apply controlled Hamiltonian evolutions $e^{-iH\tau t_0/T}$.
3. QFT on clock → eigenvalue labels $\tilde{E}_j$.
4. Ancilla rotation: $|0\rangle \to \sqrt{1-C^2/\tilde{E}_j^2}\,|0\rangle + (C/\tilde{E}_j)\,|1\rangle$. Postselect on $|1\rangle$ → **divides** by eigenvalue.

**Eigenvalue multiplication** uses the same circuit with a different ancilla rotation:

$$|0\rangle \to \sqrt{1 - C^2 \tilde{E}_j^2}\,|0\rangle + C\tilde{E}_j\,|1\rangle.$$

Postselecting on $|1\rangle$ multiplies the amplitude of each eigenvector by $\tilde{E}_j$, implementing $H|\psi\rangle$ without applying $H$ directly as a unitary (which would require a different construction).

**Why this matters:** When $H = I(F)$ is the isometry embedding of a non-unitary rectangular matrix $F$, there is no obvious way to apply $F$ to a quantum state directly. Phase estimation gives you eigenvalue access, and the ancilla rotation converts that into eigenvalue multiplication — effectively computing $I(F)|\mathbf{y}\rangle$.

## When to reach for it

- You need to apply a Hermitian operator $H$ to a state (as a map, not as time evolution) and you have efficient [[Hamiltonian simulation]] for $e^{iHt}$.
- You need to implement a non-unitary linear map via its block-embedding (e.g., [[Rectangular Matrix Pseudoinverse via Hermitian Isometry Embedding]]).
- More generally: any eigenvalue transformation $f(E_j)$ where $f$ is a polynomial or smooth function can be implemented this way; [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] generalises this to arbitrary polynomial transforms.

## Complexity

- $O(T)$ controlled evolutions of $e^{-iHt}$, where $T \sim \kappa/\varepsilon$ is the clock size needed to resolve eigenvalues to precision $\varepsilon/\kappa$.
- Success probability $O(C^2 \langle \psi | H^2 | \psi \rangle) \leq O(1/\kappa^2)$ after postselection.
- Boost to constant success via $O(\kappa)$ rounds of [[Standard Amplitude Amplification|amplitude amplification]].
- Net: $\tilde{O}(\kappa/\varepsilon)$ simulations of $e^{iHt}$.

## Caveat

- The $O(\kappa)$ amplification factor propagates to give $\kappa^4$–$\kappa^6$ in the full algorithm (if composed with a second HHL inversion step).
- QSVT handles this more cleanly: polynomial transforms of singular values can be implemented with near-optimal dependence on all parameters.
- Only works for Hermitian $H$. For non-Hermitian operators, use the isometry embedding first.

## Related notes
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Rectangular Matrix Pseudoinverse via Hermitian Isometry Embedding]]
- [[Standard Amplitude Amplification]]
- [[Amplitude Estimation via Phase Estimation on Q]]
