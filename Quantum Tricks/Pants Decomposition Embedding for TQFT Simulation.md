# Pants Decomposition Embedding for TQFT Simulation

> **Source:** Freedman, Kitaev, Wang, arXiv:quant-ph/0001071
> **Tags:** #trick #TQFT #topological #simulation #modular-functor

## What it does

Embeds the state space $V(\Sigma)$ of a topological quantum field theory — which has no natural tensor product structure — into a tensor product space $W = X^{\otimes k}$ that a quantum computer can work with.

## The trick

A surface $\Sigma$ can be cut into $k$ three-punctured spheres ("pants") along simple closed curves ("cuffs"). Each pants piece contributes one tensor factor $X = \bigoplus_{(a,b,c) \in L^3} V_{abc}$ to the computational space $W = X^{\otimes k}$.

The gluing axiom of the modular functor provides the embedding:

$$i_D: V(\Sigma) \hookrightarrow X^{\otimes k}$$

where $D$ is the pants decomposition. $V(\Sigma)$ sits as a direct summand of $W$ — the summand selected by the label-matching conditions on the cuffs.

**Why it works:** Topological operations (Dehn twists, braid moves) along cuff curves of the current decomposition act on a single tensor factor — they're local gates. Operations along non-cuff curves require changing the decomposition first (via [[F-Move and S-Move as Quantum Gates|F- and S-moves]]), which are also local gates.

**Cost:** Changing between two decompositions takes $O(\log b_1(\Sigma))$ F/S moves, because the dual graph of a well-chosen decomposition has logarithmic diameter.

## When to reach for it

- Simulating anyonic/topological systems on a standard quantum computer
- Whenever a physical state space lacks tensor product structure but has a combinatorial decomposition into pieces that do factor
- More generally: encoding a representation of a group (mapping class group) into a representation on a tensor product space, then simulating group elements as gate sequences

## Complexity

Simulation of a diffeomorphism of word length $n$ costs $O(n \cdot \log b_1(\Sigma))$ qupit gates. Each gate acts on at most 2 tensor factors.

## Caveat

The embedding is not canonical — it depends on the choice of pants decomposition. Different decompositions give different (unitarily related) embeddings. The simulation must track which decomposition is current and compose change-of-basis transformations accordingly.

## Related notes
- [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]]
- [[F-Move and S-Move as Quantum Gates]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
