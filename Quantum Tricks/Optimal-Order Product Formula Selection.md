# Optimal-Order Product Formula Selection

## What it does

For a fixed simulation problem (Hamiltonian $H$, time $t$, error $\epsilon$), the Suzuki $2k$th-order product formula has total cost that is not monotone in $k$. Higher order reduces the number of Trotter segments, but each segment contains many more exponentials. This trick estimates the order that balances those effects.

## The trick

For a $2k$th-order Suzuki formula, the number of exponentials per segment scales like $5^{k}$ up to problem-dependent factors. A typical worst-case segment-count estimate has the form
$$
r \approx
\Lambda t
\left(\frac{C_k \Lambda t}{\epsilon}\right)^{1/(2k)},
$$
or, ignoring constants that do not affect the main order optimization,
$$
\text{cost}(k)
\propto
\Lambda t \cdot 5^k
\exp\!\left(\frac{L}{2k}\right),
\qquad
L=\log(C_{\mathrm{eff}}\Lambda t/\epsilon).
$$

Taking the logarithm of the overhead gives the balance
$$
k\log 5+\frac{L}{2k}.
$$
Minimizing this expression yields
$$
k^*\approx \sqrt{\frac{L}{2\log 5}}.
$$
Thus the optimized Suzuki order grows like the square root of the logarithm of the relevant precision/time parameter, not linearly in $\log(\Lambda t/\epsilon)$.

With this optimized order,
$$
\text{cost}^*
=
\Lambda t\,
\exp\!\left(O\!\left(\sqrt{\log(C_{\mathrm{eff}}\Lambda t/\epsilon)}\right)\right),
$$
which is the standard subpolynomial precision dependence for optimized high-order product formulas.

Concrete resource estimates can still pick a moderate order. For example, the FeMo-cofactor analysis in [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes|Childs-Maslov-Nam-Ross-Su 2018]] found a Suzuki order parameter around $k\approx 6$ under its Hamiltonian, error-budget, and synthesis-cost model. That value is an empirical/resource-estimation outcome, not a consequence of a linear-in-log formula for $k^*$.

## When to reach for it

- Planning a fault-tolerant Hamiltonian simulation circuit and choosing among product-formula variants.
- Resource estimation for Hamiltonians with large effective $\Lambda t/\epsilon$ or commutator-bound analogues of that parameter.
- Comparing standard Suzuki formulas against optimized Yoshida formulas, processed formulas, corrected formulas, or interaction-picture product formulas.

## Complexity

At the optimized order, high-order product formulas have nearly linear time dependence and subpolynomial precision overhead:
$$
O\!\left(\Lambda t\,
\exp(O(\sqrt{\log(\Lambda t/\epsilon)}))\right),
$$
with the understanding that $\Lambda$ may be replaced by a sharper commutator or problem-dependent scale in refined analyses.

This should be compared carefully with LCU/QSP-style algorithms. Those algorithms usually scale with a block-encoding normalization $\alpha$ as
$$
O\!\left(\alpha t+\frac{\log(1/\epsilon)}{\log\log(1/\epsilon)}\right)
$$
queries in the standard qubitization model, plus the cost of implementing the block encoding. Product formulas may still win in resource estimates when direct term exponentials are much cheaper than block-encoding primitives.

## Caveat

- The $5^k$ prefactor comes from standard Suzuki recursion; Yoshida/Neri, optimized, or processed formulas can have different constants.
- The effective $L$ includes hidden constants, commutator scales, synthesis budgets, and allowed error allocation. These can shift the best integer order substantially.
- Worst-case norm bounds can be pessimistic. [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Commutator-based bounds]] and eigenvalue-error estimates change the effective scale and therefore the optimal order.
- Very high orders can lose in practice because coefficient sizes, formula length, synthesis precision, and implementation constants grow.

## Related notes

- [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes]] — concrete resource estimate with a moderate optimal Suzuki order
- [[Suzuki Order as a Tunable Knob for Time Scaling]] — closely related vault trick card
- [[Error Budget Splitting for Trotter-Plus-Synthesis Circuits]] — companion optimization
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — theory of high-order product formulas
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — improved Trotter error bounds that sharpen the optimal-order calculation
- [[Product Formulas]] — concept hub
- [[Bernoulli Polynomial Kernel Expansion for Product Formulas]] — alternative formula construction
