# F-Move and S-Move as Quantum Gates

> **Source:** Freedman, Kitaev, Wang, arXiv:quant-ph/0001071
> **Tags:** #trick #TQFT #topological #recoupling #6j-symbol

## What it does

Translates the combinatorial moves between pants decompositions of a surface into quantum gates, enabling simulation of topological theories on a standard quantum computer.

## The trick

Two elementary moves change one pants decomposition into another:

**F-move (fusion move / 6j-move):** Acts on a 4-punctured sphere (two adjacent pants sharing a cuff). Replaces one internal cuff with another. Under the modular functor:

$$F_{abcd}: \bigoplus_{x \in L} V_{xab} \otimes V_{\hat{x}cd} \to \bigoplus_{y \in L} V_{ybc} \otimes V_{\hat{y}da}$$

This is a unitary map on two tensor factors — a 2-qupit gate. The matrix entries are the 6j-symbols (or F-matrices) of the theory.

**S-move:** Acts on a punctured torus. Changes between two non-isotopic cuff curves. Under the functor:

$$S_a: \bigoplus_{x \in L} V_{ax\hat{x}} \to \bigoplus_{y \in L} V_{ay\hat{y}}$$

This is a unitary map on a single tensor factor — a 1-qupit gate. The matrix entries are the modular S-matrix.

**Together:** Any two pants decompositions of a surface are connected by a sequence of F, S, and M moves (where M is a Dehn twist within a single pair of pants — also a 1-qupit gate). This is a theorem of Hatcher-Thurston.

## When to reach for it

- Simulating topological theories (Chern-Simons, TQFTs) on quantum hardware
- Computing invariants of 3-manifolds by decomposing bordisms into elementary pieces
- Any situation where a computation on a topologically-defined space can be reduced to navigating between combinatorial decompositions

## Complexity

Each F-move is one 2-qupit gate; each S-move is one 1-qupit gate. The number of moves to navigate between decompositions is $O(\log b_1(\Sigma))$ for the standard logarithmic-depth decomposition of Freedman-Kitaev-Wang. The qupit dimension $p = \dim(X)$ depends on the theory.

## Caveat

The F- and S-matrices must be extended from their natural domain (the relevant summand of $W$) to unitary maps on the full tensor factor. This extension is a choice — it doesn't affect the simulation on the invariant subspace $V(\Sigma)$, but different extensions give different circuits.

## Related notes
- [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]]
- [[Pants Decomposition Embedding for TQFT Simulation]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
