
> **Tags:** #trick #graph-structure #gauge #hamiltonian-simulation
> **Source:** arXiv:0908.4398, Section 5

## What it does

For a Hamiltonian whose interaction graph is a tree (or can be decomposed into a bounded number of trees via arboricity), removes all complex phases from off-diagonal entries using a diagonal unitary transformation, reducing $\|\text{abs}(H)\|$ to $\|H\|$.

## The construction

Let $H$ have an interaction graph $G$ that is a tree rooted at vertex $r$. For each vertex $v$, define the path phase $\phi_v = \arg\prod_{(u,w)\in\text{path}(r,v)} H_{uw}$. Setting $D = \text{diag}(e^{i\phi_v})$ and transforming $H \to D H D^\dagger$ makes all off-diagonal entries nonnegative real.

On a tree, this can always be done: there are no cycles, so the product of phases along any path is well-defined (no ambiguity from loops). After the transformation, $\text{abs}(H) = H$ (entry-wise), so $\|\text{abs}(H)\| = \|H\|$.

## Why trees specifically

On a graph with cycles, the product of phases around a cycle is a gauge-invariant quantity (magnetic flux analog). You can't simultaneously make all edge entries nonneg real if there are cycles with nonzero total phase. The transformation works globally only for trees and forests.

## Application: arboricity

The arboricity $a(G)$ of a graph is the minimum number of forests needed to cover all its edges (Nash-Williams 1964). Childs–Kothari show that Hamiltonians with arboricity $a$ can be decomposed into $a$ tree-like components, each gauge-cleaned, and simulated with cost scaling with $a$ rather than $N$. This gives efficient simulation for, e.g., planar graphs ($a \le 3$) and other sparse or structured interaction graphs. See also [[Edge-Coloring Decomposition for Sparse Hamiltonians]] for a related decomposition strategy.

## Caveats

- Only applicable when the graph structure is known (non-black-box situation, or when the oracle reveals the graph).
- Does not help for general dense matrices or those with nontrivial cycle structure.
- The gauge transformation is a classical precomputation, not a quantum circuit cost.

## Related notes
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]]
- [[Norm-Gap Obstruction between H and abs(H) in Oracle Simulation]]
