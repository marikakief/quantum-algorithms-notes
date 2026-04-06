# Operator Differential Method for Commutator Product Formulas

> **Source:** Chen, Childs, Hafezi, Jiang, Kim, Xu, arXiv:2111.12177; originally from Hatano-Suzuki (2005)
> **Tags:** #trick #product-formula #commutator #algebraic-method

## What it does
Systematically derives the coefficients of a [[Product Formulas|product formula]] by expanding the product of exponentials as $e^{\Phi(x)}$ and reading off $\Phi(x)$ order by order in $x$.

## The trick
Define the operator differential $\delta_A O := [A, O]$. Then for a product $e^{p_1 xA} e^{p_2 xB} \cdots e^{p_N xB} = e^{\Phi(x)}$, the exponent is:

$$\Phi(x) = \sum_{k=0}^{\infty} \frac{(-1)^k}{k+1} \int_0^x \left(\prod_j e^{p_j t \delta_{A \text{ or } B}} - 1\right)^k \cdot \left(\sum_j p_j \cdot (\text{appropriate operator})\right) dt.$$

For a 6-gate formula, this reduces $\Phi(x)$ to a polynomial in five derived quantities $l, m, q, r, s$ (specific symmetric polynomials in the $p_i$). Setting conditions on these quantities — e.g., $l = m = 0$, $q = -1$, $r = s = 0$ for a third-order commutator formula — yields polynomial equations whose solutions give the formula coefficients.

## When to reach for it
- Constructing new [[Product Formulas|product formulas]] for commutators or sums at a specific order
- Verifying that a proposed formula achieves the claimed error order
- Deriving [[Order-Condition Cancellation in Product Formulas|order conditions]] for custom decompositions (e.g., sum-and-commutator combinations)

## Complexity
The method itself is a classical computation. The resulting formulas have gate counts determined by the number of exponentials $N$ and the recursion structure.

## Caveat
The polynomial equations become hard to solve analytically at high orders. The paper solves them for 3rd order (6 unknowns, 5 equations) but notes that 4th order remains open for direct construction. Recursive methods bypass this by composing lower-order solutions.

## Related notes
- [[Efficient Product Formulas for Commutators and Applications to Quantum Simulation (Chen-Childs-Hafezi-Jiang-Kim-Xu 2022) — Paper Notes]]
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Recursive Group-Commutator Product Formula]]
