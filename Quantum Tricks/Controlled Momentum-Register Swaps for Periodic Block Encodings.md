
> **Source:** Rubin, Berry, Malone, White, Khattar, DePrince, Sicolo, Kühn, Kaicher, Lee, Babbush, arXiv:2302.05531
> **Tags:** #trick #block-encoding #periodic-systems #qubitization #circuit-primitive #data-movement

## What it does

Coherently routes qubits for a specific crystal momentum $k$ (or $k \ominus Q$) into a fixed set of $N$ working registers where basis rotations ([[Givens Rotation Slater Determinant Preparation|Givens rotations]]) are applied. This is necessary in periodic block encodings where the SELECT oracle must act on bands belonging to different k-points depending on the prepared superposition state.

## The trick

In a periodic system with $N_k$ k-points and $N$ bands per cell, the system register has $N_k N$ qubits (per spin). The DF and THC block encodings require applying a sequence of Givens rotations to bands at a *specific* momentum $k$ — but $k$ is in superposition (prepared by PREPARE). You can't hard-wire which $N$ qubits the Givens rotations act on.

The solution uses a multiplexed controlled swap:

1. A register holds the value of $k$ (in superposition)
2. Using [[Unary Iteration|unary iteration]] on $k$, swap the $N$ system qubits for band indices at momentum $k$ into a fixed set of $N$ "working" qubits
3. Apply the Givens rotations on the working qubits
4. Swap back

The controlled swap of $N$ qubits conditioned on $N_k$ values of $k$ costs $O(N_k N)$ Toffolis (one Toffoli per controlled SWAP gate, iterated over all $k$ values via unary iteration). This is done for both $k$ and $k \ominus Q$ (which must be computed from $k$ and $Q$ via modular subtraction), and for both spin sectors — but the spin swap is controlled by a single qubit, not iterated.

The modular subtraction $k \ominus Q$ costs $O(\log N_k)$ Toffolis when dimensions are powers of two, or $O(2\log N_k)$ with a correction step for general grid sizes.

**Where this dominates:** For THC and DF, the unary-iteration-based swap costs $O(N_k N)$ per application, and it's applied $O(1)$ times per walk step. This matches or exceeds the QROAM cost for THC (which is also $O(N_k N)$ after optimization), making data movement the fundamental cost floor. No amount of symmetry exploitation can reduce it below $O(N_k N)$, because you need to physically touch every qubit at least once.

## When to reach for it

- Any [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] simulation of a periodic system in a second-quantized local-orbital basis where the SELECT oracle must apply basis-dependent operations (rotations, Pauli strings) to k-dependent subsets of the system register
- When basis rotations are loaded from [[QROM (Quantum Read-Only Memory)|QROM]] and need to act on $N$ qubits selected by a momentum quantum number in superposition
- The same pattern appears whenever you have a tensor product of symmetry sectors and need to conditionally route data

## Complexity

- Toffoli cost per swap: $O(N_k N)$ (via unary iteration)
- Called $O(1)$ times per walk step
- Modular subtraction for $k \ominus Q$: $O(\log N_k)$ Toffolis
- This sets an $\Omega(N_k N)$ lower bound on the per-step cost of any second-quantized periodic block encoding that requires basis rotations

## Caveat

This is fundamentally a data-movement cost, not a computational one. It doesn't compute anything about the Hamiltonian — it just puts the right qubits in the right place. For molecular systems (single k-point), this reduces to $O(N)$ and is negligible. For periodic systems, it becomes the dominant cost for THC and a significant fraction for DF.

Reducing this cost would require a fundamentally different approach — either encoding the system so that k-dependent operations can be applied without routing (e.g., some form of native momentum-space encoding), or finding block encodings that avoid per-k basis rotations entirely.

## Related notes
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[Unary Iteration]] — the circuit primitive driving the controlled swap
- [[Givens Rotation Slater Determinant Preparation]] — the rotations applied after the swap
- [[QROM-Loaded Givens Rotation Networks]] — loading rotation angles
- [[Symmetry-Adapted Block Encoding via QROAM Data Reduction]] — the QROAM savings that this cost eventually dominates
- [[k-Point THC Factorization (Bloch Orbital THC)]]
- [[Fermionic Swap Network]] — related data-movement primitive for molecular Trotter steps
