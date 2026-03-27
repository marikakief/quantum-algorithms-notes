
> **Source:** Aaronson & Gottesman, arXiv:quant-ph/0406196 (2004)
> **Tags:** #trick #clifford #canonical-form #circuit-compilation #gate-synthesis

## What it does

Puts any Clifford (stabilizer) circuit into an 11-round alternating form by commuting gates of the same type together. Combined with CNOT-layer minimisation, gives an $O(n^2/\log n)$-gate circuit equivalent to any Clifford unitary on $n$ qubits.

## The trick

Clifford gates satisfy simple commutation relations:
- $H$ and $P$ commute through CNOT layers (picking up at most a sign), so $H$-type gates can be sorted to specific positions.
- Phase gates commute through CNOTs (with controlled-$Z$ corrections that can be absorbed into adjacent Phase layers).

By repeatedly commuting gate types through each other, any Clifford unitary can be rewritten in the 11-round form:
$$H - C - P - C - P - C - H - P - C - P - C$$
where $H$ = layer of single-qubit Hadamards, $C$ = CNOT-only layer, $P$ = Phase-only layer.

Each CNOT layer implements a linear map over $\mathbb{F}_2^n$. By the Patel-Markov-Hayes result, any such map can be implemented with $O(n^2/\log n)$ CNOT gates. With 5 CNOT layers and 4 single-qubit layers, the total gate count is $O(n^2/\log n)$.

Using known CNOT parallelisation results, this can be achieved with $O(n^2)$ gates and $O(\log n)$ parallel depth.

## When to reach for it

- Clifford circuit compilation: any Clifford unitary has a canonical form that is essentially size-optimal.
- Circuit optimisation: bringing a Clifford to canonical form then re-minimising CNOT layers.
- Theoretical proofs: when you need to bound the circuit complexity of an arbitrary Clifford operation (e.g. in fault-tolerant overhead arguments).

## Complexity

$O(n^2/\log n)$ gates total (essentially optimal by counting: there are $2^{O(n^2)}$ Clifford unitaries, so you need $\Omega(n^2/\log n)$ gates to distinguish them).

Parallel depth: $O(\log n)$ using parallelised CNOT layers.

## Caveat

The canonical form is for *unitary* Clifford circuits only. Mid-circuit measurements and classically-controlled corrections do not fit directly (though one can defer measurements first). Also, the constant factor hidden in $O(n^2/\log n)$ from the Patel-Markov-Hayes bound may not be small in practice — the canonical form is theoretical; real compilers use heuristics.

## Related notes
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Destabilizer Tableau for Stabilizer Simulation]]
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]]
