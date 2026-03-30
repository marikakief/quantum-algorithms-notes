# Anyon Fusion Channel Encoding

> **Source:** Freedman, Kitaev, Larsen, Wang, arXiv:quant-ph/0101025
> **Tags:** #trick #topological #anyon #encoding #TQFT

## What it does
Encodes a qubit in the internal fusion degrees of freedom of a group of non-abelian anyons, producing a logical qubit with no local order parameter — the qubit state cannot be determined by any measurement on individual anyons.

## The trick
In the $\mathrm{SU}(2)$ Chern-Simons theory at level $k = 3$ (denoted $\mathrm{CS}_5$), there are particle types $\{0, 1, 2, 3\}$. Two type-1 anyons can fuse to either type 0 or type 2. Group $2n$ type-1 anyons into $n/2$ batches of 4. Within each batch:

- Fix a pair of anyons. Their fusion outcome (type 0 or type 2) encodes the qubit: $|0\rangle$ ↔ type 0, $|1\rangle$ ↔ type 2
- Constrain the batch's total fusion to type 0 (the "computational summand")
- The full computational space is spanned by $n/2$-bit strings of 0's and 2's on the intermediate fusion channels

Initialisation: pull anyon pairs from the vacuum (which always gives type 0), then use local holonomic measurements to keep or discard pairs until a known initial state is prepared.

Computation: braid anyons around each other. The braiding matrices, from the Jones representation of the braid group, act as quantum gates on the encoded qubits. By the Freedman-Larsen-Wang universality result, any unitary on the computational space can be approximated by braids.

Readout: measure the fusion channel of the leftmost composite particle (type 0 vs type 2), analogous to measuring the first qubit in the circuit model.

## When to reach for it
- When designing a topological quantum computer based on non-abelian anyons
- When you need an encoding with **inherent decoherence protection**: the qubit state has no local signature, so local noise cannot distinguish $|0\rangle$ from $|1\rangle$
- When the physical system supports anyonic excitations with the right fusion rules (e.g., Read-Rezayi states at $\nu = 12/5$ or $13/5$)

## Complexity
The encoding requires 4 anyons per logical qubit. The code space dimension grows as $\text{Fibonacci}(2n)$ for $2n$ anyons, so the qubit encoding uses a computational summand of dimension $2^{n/2}$ embedded in a larger Hilbert space. Gate synthesis via braiding has $\mathrm{polylog}(1/\varepsilon)$ overhead from the [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev theorem]].

## Caveat
- Requires non-abelian anyons. Abelian anyons (like in the $\nu = 1/3$ state or the toric code) only give phase gates and cannot generate entanglement between encoded qubits.
- The Ising anyons ($k = 2$, relevant for $\nu = 5/2$) give a $2^{n-1}$-dimensional space but the braiding is **not** universal — it generates only the Clifford group. Universality requires $k \geq 3$, $k \neq 4$.
- Maintaining well-separated anyons and performing holonomic measurements on fusion channels are major experimental challenges.

## Related notes
- [[Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003) — Paper Notes]]
- [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes]]
- [[Topological Code Space as Physical Error Protection]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
