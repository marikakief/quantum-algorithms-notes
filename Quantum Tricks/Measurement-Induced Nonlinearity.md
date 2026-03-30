# Measurement-Induced Nonlinearity

> **Source:** E. Knill, R. Laflamme, G. J. Milburn, arXiv:quant-ph/0006088
> **Tags:** #trick #linear-optics #measurement #post-selection #KLM

## What it does

Creates effective nonlinear operations (like conditional phase shifts) from purely linear dynamics by using ancilla preparation, interference, and post-selected measurement.

## The trick

Suppose you need a nonlinear transformation — one where different Fock states pick up different phases — but your Hamiltonian is quadratic (linear optics). You can't get there unitarily with beam splitters alone, because linear optics preserves the Gaussian structure of the state.

Instead:
1. Prepare an ancilla system in a known state
2. Let the input and ancilla interact via linear optics (a beam splitter network)
3. Measure the ancilla
4. Post-select on a specific measurement outcome

The conditional transformation on the unmeasured mode is diagonal in the Fock basis but **non-trivially so** — different photon numbers pick up different coefficients. By choosing the beam splitter network appropriately, you can engineer any desired set of Fock-state-dependent amplitudes (subject to the success probability constraint).

The KLM NS$_1$ gate is the simplest example: one ancilla photon, a $3 \times 3$ unitary, measurement with acceptance on $|10\rangle$ ancilla outcome. The result is $|0\rangle + |1\rangle - |2\rangle$ on the signal mode (up to normalisation), with success probability $1/4$.

The mechanism generalises: any unitary on modes + post-selection on ancilla modes implements a non-unitary (but heralded) operation on the remaining modes. The "nonlinearity" comes from projecting out specific photon-number sectors, which breaks the Gaussian structure.

## When to reach for it

- When your physical system only supports linear (quadratic-Hamiltonian) dynamics but you need nonlinear operations
- When you have access to ancilla preparation and measurement but not direct nonlinear interactions
- Quantum optics and photonic quantum computing are the primary application, but the principle applies to any bosonic system
- When non-determinism is acceptable (or can be promoted to near-determinism via [[Offline Resource State Preparation for Gate Teleportation]])

## Complexity

Success probability is $O(1)$ per gate but with a constant less than 1 (e.g., $1/4$ for NS$_1$, $1/16$ for non-deterministic c-sign). Combined with gate teleportation, the overhead becomes $O(1)$ amortised over state preparation.

## Caveat

The operation is inherently probabilistic — you only learn whether it worked *after* measuring the ancilla. Without a strategy for handling failure (like [[Offline Resource State Preparation for Gate Teleportation]] or error-detecting codes), the success probability compounds exponentially badly with circuit depth.

Also, the required beam splitter network becomes increasingly sensitive to alignment and loss as the transformation becomes more complex. In practice, fabrication tolerances limit the achievable gate fidelity.

## Related notes
- [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes]]
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]]
- [[Offline Resource State Preparation for Gate Teleportation]]
- [[Detected-Error Threshold Boosting]]
