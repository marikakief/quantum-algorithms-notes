> **Source:** Babbush, Wiebe, McClean, McClain, Neven, Chan, arXiv:1706.00023 (2018)
> **Tags:** #trick #FFFT #Fourier-transform #fermionic #circuit-compilation #planar-lattice #low-depth #second-quantized

## What it does

Implements the discrete Fourier transform on fermionic mode creation operators — mapping between the [[Plane-Wave Dual Basis|plane-wave basis]] and the plane-wave dual basis — using $O(N \log N)$ gates at depth $O(N)$ on a planar qubit lattice.

## The trick

Define the fermionic DFT on creation operators:

$$b^\dagger_j = \frac{1}{\sqrt{N}} \sum_{k \in G} e^{ik \cdot r_j} a^\dagger_k$$

where $a^\dagger_k$ creates a fermion in plane-wave mode $k$ and $b^\dagger_j$ creates a fermion in dual basis mode $j$. This transform maps the diagonal kinetic representation (plane-wave basis) to the diagonal potential representation (dual basis), and vice versa.

**Circuit construction:**

The FFFT circuit is built from two types of elementary fermionic gates:

1. **Fermionic swap gate:** Exchanges orbitals $p$ and $q$ while preserving fermionic antisymmetry. On adjacent Jordan–Wigner qubits this is a 2-local gate (see [[Fermionic Swap Network]]).

2. **Two-mode fermionic mixing gate (fermionic beam splitter):** Acts as:
   $$\begin{pmatrix} a^\dagger_p \\ a^\dagger_q \end{pmatrix} \mapsto \frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix} \begin{pmatrix} a^\dagger_p \\ a^\dagger_q \end{pmatrix}$$
   This is the fermionic analogue of the Hadamard gate on the mode space; it corresponds to a Givens rotation on the single-particle Hilbert space. Combined with a phase gate $a^\dagger_q \mapsto e^{i\theta} a^\dagger_q$, the full family of DFT butterfly stages is realised.

The overall structure mirrors the Cooley-Tukey radix-2 FFT decimation-in-frequency algorithm: recursively split the $N$ modes into even and odd index groups, apply butterfly stages (Hadamard-like mixing + phase rotations), and sort with fermionic swaps. The sorting stage is what makes this a true fermionic transform (not just a bosonic/qubit DFT), because it must respect the antisymmetry encoded in the Jordan–Wigner string.

**Depth on a planar lattice:** The paper (Appendix I) proves the full 3D FFFT can be implemented in $O(N)$ depth on a 2D qubit grid, using $O(N \log N)$ gates total. The key is that the butterfly stages and swap networks can be parallelised across the 2D layout with no long-range interactions.

**Usage in simulation:**

A single Trotter step of $H = T + (U + V)$ becomes:

```
e^{-iT·τ/2}  [O(1) depth, diagonal in pw basis]
    ↓ FFFT† [O(N) depth]
e^{-i(U+V)·τ} [O(N) depth, diagonal in dual basis]
    ↓ FFFT  [O(N) depth]
e^{-iT·τ/2}  [O(1) depth]
```

Total Trotter step depth: $O(N)$.

## When to reach for it

- Simulating periodic electronic systems where the Hamiltonian is expressed in the [[Plane-Wave Dual Basis]]
- Any quantum simulation where kinetic and potential energies are diagonal in Fourier-dual bases and you need to alternate between the two
- As a subroutine in variational circuits for the uniform electron gas (jellium) on planar-lattice hardware
- Wherever a standard quantum Fourier transform is used for mode switching but fermionic antisymmetry must be preserved

## Complexity

- **Gates:** $O(N \log N)$ two-qubit gates
- **Depth:** $O(N)$ on a planar (2D grid) qubit lattice
- **Connectivity:** Requires 2D planar nearest-neighbour connectivity (not achievable with linear chain alone)
- **Comparison:** Quantum Fourier transform (bosonic) achieves $O(\log^2 N)$ depth on arbitrary connectivity; the fermionic version trades connectivity locality for $O(N)$ depth at $O(N \log N)$ gates.

## Caveat

- **Not a free operation.** The FFFT costs $O(N)$ depth per application. In a Trotter step, two FFTs are needed (forward and inverse), each $O(N)$, so the FFFT dominates the step cost.
- **Planar connectivity required.** The $O(N)$ depth result relies on 2D grid qubit layout. On a linear chain, depth increases.
- **Jordan–Wigner specific.** The construction uses the fermionic swap to maintain the JW canonical ordering. It doesn't directly generalise to Bravyi-Kitaev encoding.
- **Sorting overhead.** The swap network required to move modes to the correct positions for butterfly stages contributes an $O(N)$ overhead in gate count. This is what pushes the gate count above the classical $O(N \log N)$ of an FFT (which performs no swaps).
- **Phase rotations must be calibrated.** The twiddle factors $e^{i\theta}$ in the butterfly stages depend on the mode indices and must be compiled correctly. For 3D systems ($N = N_x N_y N_z$), the multi-dimensional DFT factorises, but the fermionic ordering across dimensions requires care.

## Related notes
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]]
- [[Plane-Wave Dual Basis]]
- [[Fermionic Swap Network]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Trotterized Time Evolution for Chemistry]]
- [[Truncated Taylor Series Simulation]]
