# Dihedral HSP Query Samples vs Postprocessing

## Pattern

Separate two questions that are often conflated in HSP algorithms:

- **Query/sample complexity:** how many oracle-generated coset states identify the hidden subgroup information-theoretically?
- **Extraction complexity:** how hard is it to process those states or their measurement outcomes into generators?

For dihedral HSP, the first answer is polynomial, even logarithmic in early formulations. The second answer is the hard one.

## Use case

This lens is useful when a paper claims progress on nonabelian HSP. Ask which part improved:

1. fewer oracle queries;
2. a better measurement on one or many coset states;
3. faster classical post-processing;
4. lower quantum memory;
5. a promise restriction on the hiding function or subgroup family.

## Why it matters

Ettinger--Høyer already showed a linear-query algorithm for dihedral HSP, but their post-processing is exponential. EHK later shows polynomial query complexity for every finite group. Kuperberg and Regev attack the extraction side for dihedral HSP by sieving.

## Source papers

- [[On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998) — Paper Notes|Ettinger--Høyer (1998)]].
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger--Høyer--Knill (2004)]].
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]].
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]].
