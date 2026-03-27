
> **Source:** Babbush, McClean, Wecker, Aspuru-Guzik, Wiebe, arXiv:1410.8159 (PRA 2015)
> **Tags:** #trick #trotter-error #quantum-chemistry #error-mitigation #classical-processing

## What it does

Reduces the effective Trotter error in a quantum chemistry simulation by computing the error on a classical wavefunction ansatz and subtracting it from the quantum result — at zero additional quantum cost.

## The trick

The Trotter error in the ground-state energy is $\Delta E_0 = \langle\psi_0|V^{(1)}|\psi_0\rangle$, where $V^{(1)}$ is the leading BCH error operator. This can't be computed classically (it requires the exact ground state). But you *can* compute $\Delta E_{\text{CI}} = \langle\psi_{\text{CI}}|V^{(1)}|\psi_{\text{CI}}\rangle$ using a classical CI ansatz (Hartree-Fock, CISD, CISDT, etc.).

**Step 1:** Classically compute $V^{(1)}$ from the Hamiltonian coefficients (cost: polynomial, $O(N^{10})$ terms to enumerate, then normal-order).

**Step 2:** Evaluate $\langle\psi_{\text{CI}}|V^{(1)}|\psi_{\text{CI}}\rangle$ on a truncated CI wavefunction.

**Step 3:** Subtract this from the quantum simulation result: $E_{\text{corrected}} = E_{\text{quantum}} - \Delta E_{\text{CI}}$.

The corrected result has residual error $|\Delta E_0 - \Delta E_{\text{CI}}|$, which is typically an order of magnitude smaller than $|\Delta E_0|$ alone (and much smaller than $\|V^{(1)}\|$).

**Bonus:** $\Delta E_{\text{CI}}$ also serves as an a priori *estimate* of how many Trotter steps are needed — far more realistic than the operator norm bound.

## When to reach for it

- Any Trotter-based quantum chemistry simulation where you want to reduce step count or improve accuracy without additional quantum resources.
- Resource estimation: use the classical ansatz error to predict the required $\mu$ instead of the operator norm.
- When CISD or CISDT gives a decent approximation to the ground state (true for most single-reference systems).

## Complexity

Classical cost: constructing $V^{(1)}$ is $O(N^{10})$ in the number of terms (polynomial, but large). Evaluating on a CISD state is $O(N^4)$ determinants. The quantum simulation itself is unchanged — no extra qubits, no extra gates.

## Caveat

Requires a good classical ansatz. For strongly correlated systems where CISD is qualitatively wrong (e.g., near dissociation, transition metal clusters), the subtraction may not help and could even introduce bias. The approach also assumes the leading-order BCH term $V^{(1)}$ dominates — if higher-order terms matter (very large $\Delta t$), the subtraction underperforms.

## Related notes
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]]
- [[Nuclear Charge Scaling of Trotter Error]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Order-Condition Cancellation in Product Formulas]]
