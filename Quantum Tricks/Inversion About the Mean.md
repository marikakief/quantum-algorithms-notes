
> **Source:** Grover, arXiv:quant-ph/9605043 (1996)
> **Tags:** #trick #grover #reflection #amplitude-amplification #fundamental

## What it does

Reflects the amplitude of every basis state about the average amplitude across all states. States above the mean go below by the same amount, and vice versa.

## The trick

Given a reference state $|\psi\rangle = \frac{1}{\sqrt{N}}\sum_i |i\rangle$ (the uniform superposition), the diffusion operator is:

$$
D = -I + 2|\psi\rangle\langle\psi|
$$

In matrix form: $D_{ij} = 2/N$ for $i \neq j$, and $D_{ii} = -1 + 2/N$.

Acting on an arbitrary state with amplitudes $\alpha_i$, the operator maps each amplitude to:

$$
\alpha_i \mapsto 2\bar{\alpha} - \alpha_i
$$

where $\bar{\alpha} = \frac{1}{N}\sum_i \alpha_i$ is the mean amplitude. This is a reflection about $\bar{\alpha}$.

**Why it's unitary:** Write $D = -I + 2P$ where $P = |\psi\rangle\langle\psi|$ is a rank-1 projector. Since $P^2 = P$, we get $D^2 = I$, so $D$ is both unitary and its own inverse.

**Circuit implementation:** $D = H^{\otimes n} R H^{\otimes n}$, where $R$ flips the phase of every state except $|0\rangle^{\otimes n}$. Cost: $O(n)$ gates.

## When to reach for it

- **Grover search:** The diffusion step in every [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover iteration]]. After the oracle flips the marked state's sign, inversion about the mean amplifies its amplitude.
- **[[Standard Amplitude Amplification|Amplitude amplification]]:** The reflection about $A|0\rangle$ in the operator $Q = -AS_0 A^{-1}S_\chi$ is exactly inversion about the mean in the initial state's frame. The generalisation replaces $|\psi\rangle$ with $A|0\rangle$.
- **Any "two-reflection" algorithm:** Whenever you need a reflection about a known state. The [[Qubitization Iterate|qubitization iterate]] and [[Phased Qubitization Sequence|phased qubitization]] use the same structure with different reference states.
- **Fixed-point search:** Modified versions (replacing the $\pi$ phase with other angles) give [[Fixed-Point Amplitude Amplification with Eigenstate Filtering|fixed-point amplification]] that converges monotonically.

## Complexity

$O(n) = O(\log N)$ gates per application: two layers of Hadamards plus a multi-controlled phase gate.

## Caveat

The operator depends on the reference state $|\psi\rangle$. In Grover search this is the uniform superposition, but in [[Standard Amplitude Amplification|amplitude amplification]] it's $A|0\rangle$ — if $A$ is expensive, so is each reflection. Also, multiple applications oscillate rather than converge: $O(\sqrt{N})$ iterations is optimal, but more iterations *reduce* the success probability. Fixed-point versions address this at some cost in circuit depth.

## Related notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the original paper
- [[Standard Amplitude Amplification]] — the generalisation to arbitrary initial algorithms
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — the full framework paper
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]] — non-oscillating variant
- [[QSVT Meta-Template]] — inversion about the mean as a special case of quantum signal processing
- [[Qubitization Iterate]] — uses the same two-reflection structure with block-encoded Hamiltonians
