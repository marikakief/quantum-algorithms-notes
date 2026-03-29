# Jørgensen Inequality for Non-Unitary Universality Seeding

> **Source:** Aharonov, Arad, Eban & Landau, arXiv:quant-ph/0702008 (Section 9); Jørgensen (1976); Sullivan (1985)
> **Tags:** #trick #universality #non-unitary #SL2 #Möbius-transformations

## What it does

Proves that two non-unitary matrices in $SL(2, \mathbb{C})$ generate a dense subgroup, by combining Jørgensen's inequality (from Möbius transformation / Kleinian group theory) with Sullivan's structure theorem for non-discrete subgroups.

## The trick

In unitary universality proofs (e.g., [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes|Aharonov-Arad]]), you show two generators produce an infinite subgroup of $SU(2)$, which must then be dense (by the classification of finite subgroups of $SO(3)$). For $SL(2, \mathbb{C})$, finite subgroups aren't the only obstacle — the group could be infinite but *discrete* (a Kleinian group), which is not dense.

**Jørgensen's inequality** (1976): If $X, Y \in SL(2, \mathbb{C})$ generate a **non-elementary, discrete** group, then:

$$|\text{Tr}^2(X) - 4| + |\text{Tr}(XYX^{-1}Y^{-1}) - 2| \geq 1$$

So if the LHS is $< 1$, the group is either elementary or **non-discrete**.

**Baribeau-Ransford criterion** (2000): Check that $\langle X, Y \rangle$ is non-elementary by verifying that $(\tau, \tau', \gamma) = (\text{Tr}^2(X) - 4,\ \text{Tr}^2(Y) - 4,\ \text{Tr}^2([X,Y]) - 2)$ don't fall into three degenerate cases.

**Sullivan's theorem** (1985): A non-elementary, non-discrete subgroup of $SL(2, \mathbb{C})$ is either dense in $SL(2, \mathbb{C})$, or its identity component is conjugate to $SL(2, \mathbb{R})$.

Putting it together: if your generators satisfy the Jørgensen inequality violation *and* are non-elementary *and* have at least one non-real trace, the generated group is dense in $SL(2, \mathbb{C})$. If all parameters are real, you get density in $SL(2, \mathbb{R})$.

**Concrete condition for the Potts/Tutte crossing operators:** With $\alpha = q/(1 + qv_1^{-1})$, $\beta = \sqrt{1 + v_2}$:

$$\left|\frac{\alpha - 1}{\alpha}\right|^2 + \left|\frac{q-1}{q^2}\right| \cdot \left|\frac{\alpha - 1}{\alpha}\right|^2 \cdot \left|\frac{\beta - 1}{\beta}\right|^2 < 1$$

guarantees the non-unitary seeding lemma.

## When to reach for it

- Proving universality/density for non-unitary representations — whenever your generators lie in $SL(2, \mathbb{C})$ or $SL(2, \mathbb{R})$ rather than $SU(2)$
- Any quantum algorithm using non-unitary algebraic representations where you need BQP-hardness
- Establishing computational hardness of approximation problems involving partition functions or graph polynomials at non-physical parameters

## Complexity

The Jørgensen and Baribeau-Ransford checks are $O(1)$ — just compute traces and compare. Sullivan's theorem is non-constructive but gives density as a structural conclusion. Efficiency (actually constructing the approximating sequences) then follows from the [[Solovay-Kitaev Recursive Gate Compilation|non-unitary Solovay-Kitaev theorem]].

## Caveat

The conditions are sufficient but not necessary. There exist parameters where density holds but the Jørgensen inequality is not violated — the full characterisation of which parameters give dense subgroups of $SL(2, \mathbb{C})$ remains open.

The technique is specific to $SL(2, \mathbb{C})$ and its subgroups. For higher-dimensional groups, the seeding lemma bootstraps from 2D via the [[Bridge Lemma for Tensor Product Universality|Bridge Lemma]] and [[Decoupling via Iterated Commutators|Decoupling Lemma]], but the initial 2D seed relies on the special structure of Möbius transformations.

## Related notes
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes]]
- [[Bridge Lemma for Tensor Product Universality]]
- [[Decoupling via Iterated Commutators]]
- [[Solovay-Kitaev Recursive Gate Compilation]]
- [[Non-Unitary Gate Implementation via Post-Selection]]
