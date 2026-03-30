# PH-to-AC0 Correspondence for Oracle Separations

> **Source:** Furst-Saxe-Sipser (1984); Yao (1985); applied to BQP by Aaronson, arXiv:0910.4698 (2010)
> **Tags:** #trick #circuit-complexity #PH #AC0 #oracle-separation #structural

## What it does

Converts a PH oracle machine into an AC$^0$ circuit acting on the oracle string, so that AC$^0$ lower bounds (Håstad, Razborov-Smolensky, Braverman) directly yield oracle separations against PH.

## The trick

Let $M$ be a PH machine that computes a property of a $2^n$-bit oracle string $x$. Since $M$ uses $\Sigma^P_k$ or $\Pi^P_k$ computation:
- Existential quantifiers ($\exists y$) become layers of OR gates
- Universal quantifiers ($\forall y$) become layers of AND gates
- The deterministic base computation becomes the bottom-level gates

The result is an AC$^0$ circuit of depth $O(k)$ (from the $k$ levels of alternation in PH) and size $2^{\text{poly}(n)}$ (from the polynomial-length quantified strings) that computes the same function of $x$.

**Consequence:** If you prove a $2^{\omega(\text{polylog}\, n)}$ lower bound on AC$^0$ circuits computing some function of $x$, you get an oracle $A$ (whose truth table is $x$) relative to which no PH machine computes that function.

## When to reach for it

- Any time you want to construct an oracle separating some class $C$ from PH
- The standard route: (1) define a problem on exponentially long strings, (2) show a quantum/randomised/$C$ algorithm solves it with few queries, (3) prove an AC$^0$ lower bound on the string
- Especially powerful when combined with Håstad's Switching Lemma or Braverman's results on pseudorandomness fooling AC$^0$
- Used in [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes|Aaronson (2010)]] to separate FBQP from FBPP$^{\text{PH}}$ via Fourier Fishing, and conditionally BQP from PH via Fourier Checking

## Complexity

The AC$^0$ circuit has:
- Depth = number of quantifier alternations + $O(1)$
- Size = $2^{\text{poly}(n)}$ (each quantified string has polynomial length in $n$, giving OR/AND gates of fan-in $2^{\text{poly}(n)}$)

## Caveat

The correspondence is tight for oracle separations but says nothing about the unrelativised world. IP = PSPACE is the classic example of a non-relativising result — oracle separations don't preclude clever interactions. Still, for the specific purpose of providing *evidence* that one class is not contained in another, oracle separations via this correspondence are the standard tool.

## Related notes
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]]
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]
- [[Secret Bias Smuggling for Lower Bounds]]
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — related PH collapse argument
