# Depth-First Quantum Sieve to Save Quantum Space

## Pattern

A sieve that naively stores many quantum states can often be reorganized as a recursive computation. Generate child states only when needed, combine them, then discard them. The quantum register holds one branch at a time; the branch description is classical.

## HSP version

In Kuperberg's collimation sieve, the algorithm needs a tree of phase-vector combinations. Traversing that tree depth-first keeps only $O(\log N)$ quantum space while preserving subexponential time. The price is classical storage for coefficient lists and recursion data.

## When to try it

Use this when:

- the quantum state has a compact classical descriptor;
- combination operations are local in the sieve tree;
- intermediate quantum states do not need to remain coherent across far-apart branches;
- quantum memory is more expensive than classical memory.

## Source paper

- [[Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) — Paper Notes|Kuperberg (2013)]].
