# Free Sum-and-Commutator Simulation

> **Source:** Chen, Childs, Hafezi, Jiang, Kim, Xu, arXiv:2111.12177
> **Tags:** #trick #product-formula #commutator #hamiltonian-simulation

## What it does
Simulates $e^{\alpha(A+B) + \beta[A,B]}$ using the same number of gates as simulating the commutator $e^{\beta[A,B]}$ alone — the linear terms $A + B$ ride along for free.

## The trick
Construct a 6-gate product formula $f_R(x)$ that implements:

$$f_R(x) = e^{x(A+B) + Rx^2[A,B] + O(x^4)}$$

by solving the polynomial equations $l = m = 1$, $q = -R + 1/2$, $r = s = 1/6$. For large $R$ (small step size), explicit approximate solutions exist. Then set $x = \alpha/n$ and $R = \beta n / \alpha^2$ to get:

$$f_R(\alpha/n)^n = e^{\alpha(A+B) + \beta[A,B] + O((\alpha\beta + \beta^2)/n)}$$

using $6n$ gates. In the limit $\alpha \to 0$, this reduces to the pure commutator formula. An extra operator $C$ can also be included via $e^{(\alpha/n)C} f_R(\alpha/n)$ at cost of one additional gate per step (7n total).

## When to reach for it
- Counterdiabatic driving: the correction term $\dot{\lambda} C_\lambda \approx i c_1(\lambda)[H, \partial_\lambda H]$ is a commutator, and you want to simulate $H + \dot{\lambda}C_\lambda$ together
- Any [[Hamiltonian simulation]] where the target Hamiltonian decomposes as a sum plus a commutator of available terms
- Lattice models where NNN terms arise as commutators of NN terms (the sum handles the NN part simultaneously)

## Complexity
$6n$ gates for error $O(1/n)$. Same asymptotic scaling as standard first-order [[Product Formulas|Trotterization]] of a two-term sum.

## Caveat
The approximate solution (Eq. 19 of the paper) requires $R \gg 1$, i.e., many time steps. For moderate step counts, the exact polynomial equations must be solved numerically for each $R$ value. Also, the error $O((\alpha\beta + \beta^2)/n)$ means the commutator amplitude $\beta$ enters the error bound quadratically.

## Related notes
- [[Efficient Product Formulas for Commutators and Applications to Quantum Simulation (Chen-Childs-Hafezi-Jiang-Kim-Xu 2022) — Paper Notes]]
- [[Operator Differential Method for Commutator Product Formulas]]
- [[NNN Hopping from NN Commutators]]
- [[Recursive Group-Commutator Product Formula]]
