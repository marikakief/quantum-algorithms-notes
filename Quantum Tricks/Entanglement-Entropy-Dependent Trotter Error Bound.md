# Entanglement-Entropy-Dependent Trotter Error Bound

> **Source:** Zhao, Zhou, Childs, arXiv:2406.02379
> **Tags:** #trick #hamiltonian-simulation #trotter #product-formulas #entanglement #entanglement-entropy #average-case

## What it does

Interpolates between worst-case and average-case [[Product Formulas|product-formula]] error bounds by replacing adversarial alignment with a local reduced-state mixedness term on the supports of the leading Trotter-error operators.

## The trick

Let

$$
U_0(\delta t) - \mathcal U_p(\delta t) = E\,\delta t^{p+1} + \mathcal E_{\mathrm{re}},
\qquad E = \sum_j E_j.
$$

For an input state $|\psi\rangle$, the leading contribution is

$$
\Bigl\|\sum_j E_j|\psi\rangle\Bigr\|^2 = \sum_{j,j'} \langle \psi|E_j^\dagger E_{j'}|\psi\rangle.
$$

Now restrict to the support of $E_j^\dagger E_{j'}$ and call the reduced density matrix there $\rho_{j,j'}$. Then

$$
\langle \psi|E_j^\dagger E_{j'}|\psi\rangle
$$

splits into

- a maximally mixed contribution, which sums to the Frobenius-norm term $\|E\|_F^2$,
- plus a correction controlled by how far $\rho_{j,j'}$ is from $I/d_{\rho_{j,j'}}$.

This gives

$$
\|(U_0-\mathcal U_p)|\psi\rangle\|
=
O\!\left(
\delta t^{p+1}
\sqrt{\sum_{j,j'} \|E_j^\dagger E_{j'}\|\,\operatorname{Tr}|\rho_{j,j'}-I/d_{\rho_{j,j'}}|}
+
\delta t^{p+1}\|E\|_F
\right)
$$

up to the higher-order remainder term.

Using Pinsker / relative entropy,

$$
\operatorname{Tr}|\rho-I/d| \le \sqrt{2(\log d - S(\rho))},
$$

so one gets the entropy version

$$
\|(U_0-\mathcal U_p)|\psi\rangle\|
=
O\!\left(
\delta t^{p+1}
\sqrt{\sum_{j,j'} \|E_j^\dagger E_{j'}\|\sqrt{\log d_{\rho_{j,j'}}-S(\rho_{j,j'})}}
+
\delta t^{p+1}\|E\|_F
\right).
$$

If all relevant local RDMs are nearly maximally mixed, the first term collapses and the error drops to the [[Frobenius-Norm Average-Case Error Bound|average-case]] scale.

## When to reach for it

- When you know the input state is locally thermalized or highly entangled on small subsystems
- When the Hamiltonian is local, so the supports of $E_j^\dagger E_{j'}$ stay small
- When you want a bound tighter than worst case but more state-specific than a full 1-design average

## Complexity

No circuit overhead for the bound itself. It changes the step-count estimate from the worst-case $\|E\|$ scale toward the average-case $\|E\|_F$ scale when local entropy deficits are small.

## Caveat

- This is **not** a simple global Schmidt-rank bound. The relevant quantity is local mixedness on the supports of $E_j^\dagger E_{j'}$.
- If those local RDMs are not close to maximally mixed, the improvement can disappear.
- Higher-order remainder terms are still present and matter when $\delta t$ is not small.

## Related notes

- [[Entanglement Accelerates Quantum Simulation (Zhao-Zhou-Childs 2025) — Paper Notes]]
- [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]]
- [[Frobenius-Norm Average-Case Error Bound]]
- [[Frobenius-Norm Commutator Sum for Product Formulas]]
- [[k-Uniformity Convergence to Average-Case Error]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
