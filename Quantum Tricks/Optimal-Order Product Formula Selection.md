# Optimal-Order Product Formula Selection

## What it does

For a fixed simulation problem (Hamiltonian $H$, time $t$, error $\epsilon$), the Suzuki $2k$th-order product formula has total simulation cost that is not monotone in $k$: there is an optimal order $k^*$ that minimizes T-gate count (or query complexity). This trick describes how to find $k^*$ and why higher order formulas often win in practice.

## The trick

The $2k$th-order Suzuki formula requires $5^{k-1}$ exponentials per step (for a two-term splitting) or scales similarly for multi-term Hamiltonians. The number of steps $r$ to achieve Trotter error $\epsilon_T$ satisfies

$$r = \Theta\!\left( \frac{(\Lambda t)^{1+1/(2k)}}{\epsilon_T^{1/(2k)}} \right)$$

The total cost (ignoring synthesis) is proportional to $r \cdot 5^{k-1}$. Minimizing over $k$ gives the crossover where the saving in $r$ from higher order compensates for the multiplicative cost $5^{k-1}$ per step.

Setting $\partial/\partial k [r \cdot 5^{k-1}] = 0$ and solving approximately gives

$$k^* \approx \frac{1}{2} \cdot \frac{\ln(\Lambda t / \epsilon_T)}{\ln 5}$$

For the FeMo cofactor parameters ($\Lambda t / \epsilon_T \approx 10^{10}$ after accounting for phase estimation repetitions), this gives $k^* \approx 6$. When synthesis costs are included (adding a $\log(1/\epsilon_S)$ factor per gate), the optimum shifts slightly toward higher $k$ because synthesis cost penalizes having more steps.

The practical rule of thumb: if $\Lambda t / \epsilon \gtrsim 10^5$, go to at least 6th order (i.e., $k \geq 3$). Below $10^3$, low order ($k = 1$ or $k = 2$) wins.

From [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes|Childs-Maslov-Nam-Ross-Su 2018]], the FeMo cofactor with $k=6$ vs $k=2$ saves a factor of roughly $10^3$ in T-gate count over the second-order formula.

## When to reach for it

- Planning a fault-tolerant Hamiltonian simulation circuit and choosing among product formula variants.
- Resource estimation for quantum chemistry Hamiltonians with large $\Lambda t / \epsilon$ ratios.
- Any simulation where the Hamiltonian is expressed as a sum of individually exponentiable terms and query or gate count is the primary cost.

## Complexity

At the optimal order $k^*$:

$$T_{\text{total}}^* = O\!\left( 5^{k^*} \cdot (\Lambda t)^{1+1/(2k^*)} \epsilon^{-1/(2k^*)} \right) = O\!\left( (\Lambda t) \cdot e^{O(\sqrt{\ln(\Lambda t / \epsilon)})} \right)$$

The final expression is quasi-polynomial in the natural parameters — better than any fixed polynomial in $1/\epsilon$ but worse than the $O(\Lambda t \cdot \text{poly}\log(1/\epsilon))$ achievable by [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Taylor series LCU]] or [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]].

## Caveat

- The $5^{k-1}$ prefactor comes from the standard Suzuki recursion; different recursive schemes (e.g., the Yoshida-Neri construction) have smaller prefactors for some $k$.
- The analysis assumes all terms in $H$ contribute equally to $\Lambda$; if the Hamiltonian has a dominant term, asymmetric product formulas or interaction-picture methods can do better.
- The formula for $k^*$ uses the worst-case Trotter error bound; tighter [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|commutator-based bounds]] change the effective $\Lambda$ and therefore the optimal $k$.
- For $k > 6$ or so, the inner product formula constant (the coefficient of $5^{k-1}$) grows and the practical gain saturates; diminishing returns set in.

## Related notes

- [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes]] — source paper with concrete $k^* = 6$ example
- [[Suzuki Order as a Tunable Knob for Time Scaling]] — closely related vault trick card
- [[Error Budget Splitting for Trotter-Plus-Synthesis Circuits]] — companion optimization
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — theory of high-order product formulas
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — improved Trotter error bounds that sharpen the optimal $k$ calculation
- [[Product Formulas]] — concept hub
- [[Bernoulli Polynomial Kernel Expansion for Product Formulas]] — alternative formula construction
