# Operator Differential Method for Commutator Product Formulas

> **Source:** Chen, Childs, Hafezi, Jiang, Kim, Xu, arXiv:2111.12177; originally from Hatano-Suzuki (2005)
> **Tags:** #trick #product-formula #commutator #algebraic-method

## What it does
Systematically derives the coefficients of a [[Product Formulas|product formula]] by expanding the product of exponentials as $e^{\Phi(x)}$ and reading off $\Phi(x)$ order by order in $x$.

## The trick
Define the operator differential $\delta_A O := [A, O]$. Then for a product $e^{p_1 xA} e^{p_2 xB} \cdots e^{p_N xB} = e^{\Phi(x)}$, the exponent is:

$$\Phi(x) = \sum_{k=0}^{\infty} \frac{(-1)^k}{k+1} \int_0^x \left(\prod_j e^{p_j t \delta_{A \text{ or } B}} - 1\right)^k \cdot \left(\sum_j p_j \cdot (\text{appropriate operator})\right) dt.$$

For the alternating 6-gate ansatz
$$
e^{p_1xA}e^{p_2xB}e^{p_3xA}e^{p_4xB}e^{p_5xA}e^{p_6xB}=e^{\Phi(x)},
$$
the expansion through third order can be expressed using five derived coefficient polynomials $l,m,q,r,s$:
$$
\Phi(x)=x(lA+mB)
+\frac{x^2}{2}(lm-2q)[A,B]
+\frac{x^3}{6}\left[\left(\frac{l^2m}{2}-3r\right)[A,[A,B]]
+\left(\frac{m^2l}{2}-3s\right)[B,[B,A]]\right]
+O(x^4).
$$
Here $l=p_1+p_3+p_5$, $m=p_2+p_4+p_6$, and $q,r,s$ are the paper's corresponding higher-order polynomials in the $p_i$.

For a pure third-order commutator formula, impose
$$
l=m=0,\qquad q=-1,\qquad r=s=0.
$$
One solution is
$$
S_3(x)=
e^{\frac{\sqrt{5}-1}{2}xA}
e^{\frac{\sqrt{5}-1}{2}xB}
e^{-xA}
e^{-\frac{\sqrt{5}+1}{2}xB}
e^{\frac{3-\sqrt{5}}{2}xA}
e^{xB}
=e^{x^2[A,B]+O(x^4)}.
$$

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
