# QRACM Accounting for Quantum Search over Classical Lists

> **Source:** Laarhoven, Mosca, van de Pol, arXiv:1301.6176
> **Tags:** #trick #resource-estimation #qracm #quantum-search

## What it does

Makes the memory assumption in Groverised list algorithms explicit: query count, ordinary qubits, and quantumly addressable classical memory are separate resources.

## The trick

When a quantum algorithm searches a precomputed classical list $L=(L_0,\ldots,L_{N-1})$, the oracle usually needs the map

$$
|j\rangle|0\rangle \mapsto |j\rangle|L_j\rangle
$$

inside a reversible predicate. This does not require storing all $L_j$ as qubits, but it does require coherent indexed access to classical data: quantumly addressable classical memory.

Laarhoven--Mosca--van de Pol use this distinction for lattice sieving:

- the huge lattice-vector lists remain classical data;
- Grover search uses $O(\log N)$ ordinary qubits for the index register, plus workspace for the predicate;
- the query complexity is $O(\sqrt{N})$ list lookups;
- the classical memory must still be addressable in superposition.

This accounting is more honest than writing only "Grover gives $\sqrt{N}$". It says exactly what has to be built for the speedup to be meaningful.

## When to reach for it

Use this resource split whenever a paper claims a quantum speedup for a classical list algorithm:

1. Is the list virtual, so $L_j$ can be computed reversibly from $j$ in polylogarithmic time?
2. If the list is materialised, is QRACM assumed?
3. How many ordinary qubits does the predicate need?
4. Does the claimed time include memory lookup cost?

This is especially relevant for lattice sieves, collision search, meet-in-the-middle attacks, and cryptanalytic resource estimates.

## Complexity

For a list of size $N$, Grover search uses $O(\sqrt{N})$ oracle calls and $O(\log N)$ index qubits, plus predicate workspace. The memory footprint remains $N$ classical records, but the access model is quantum-coherent indexed lookup.

## Caveat

QRACM is not free. It may be easier than storing an exponentially large quantum state, but it is still a major physical and architectural assumption. If coherent lookup costs scale poorly, the headline square-root query saving can overstate the real advantage.

## Related notes

- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
- [[Groverised Centre Search in Lattice Sieves]]
- [[Quantum Birthday Search in Saturation SVP]]
- [[Classical Preprocessing plus Grover Search]]
