# Feynman-Kitaev Construction with Non-Negative Entries

> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, arXiv:2303.13012; builds on Jordan, Gosset, Love (2010) and Childs, Gosset, Webb (2015)
> **Tags:** #trick #BQP-completeness #Feynman-Kitaev #circuit-to-Hamiltonian #non-negative-matrix #coupled-oscillators

## What it does
Encodes a universal quantum circuit into a Hamiltonian (or spring-constant matrix) whose off-diagonal entries are all non-positive, making it a valid graph Laplacian describing a physical system of coupled oscillators.

## The trick

The standard Feynman-Kitaev construction encodes gates $U_1, \ldots, U_L$ as:

$$A' = 4I - \sum_{l=1}^L (|l\rangle\langle l+1| + |l+1\rangle\langle l|) \otimes U_l$$

The problem: Hadamard gates have negative entries ($H_{00} = H_{01} = 1/\sqrt{2}$, $H_{11} = -1/\sqrt{2}$), making some off-diagonal entries of $A'$ positive — invalid as spring constants.

**Fix:** Replace Hadamard $H$ acting on qubit $q$ with the encoded matrix acting on qubits $(q, q+1)$:

$$H_{\text{enc}} = \frac{1}{\sqrt{2}} \begin{pmatrix} I_2 & I_2 \\ I_2 & X \end{pmatrix}$$

This has all non-negative entries. The key identity:

$$(I_2 \otimes \langle -|)\, H_{\text{enc}}\, (I_2 \otimes |-\rangle) = H$$

So $H_{\text{enc}}$ acts as the Hadamard gate on the logical subspace where the ancilla qubit is $|-\rangle$. The $|-\rangle$ state absorbs the negative signs.

Toffoli and Pauli $X$ already have non-negative entries, so they're encoded trivially as $U_l \otimes I_2$.

The resulting $A$ has off-diagonal entries in $\{0, -1/\sqrt{2}, -1\}$, diagonal entries equal to 4, and is 4-sparse. It describes a valid system of coupled oscillators with $\kappa_{jk} \in \{0, 1/\sqrt{2}, 1\}$.

## When to reach for it

- Proving BQP-hardness for problems defined on physically constrained systems (non-negative couplings, spring networks, stochastic matrices).
- Constructing Hamiltonians with non-negative off-diagonal entries that still encode universal computation.
- Any setting where the computational model is restricted to "stoquastic-like" interactions but you need to embed arbitrary quantum computation.

## Complexity

The encoded circuit uses $N = (L+1) \cdot 2^{q+1}$ oscillators, where $L$ is the circuit size and $q$ the qubit count. Each gate query to $A$ is computable in $O(\text{poly}(q))$ time from the circuit description. The kinetic energy of the output oscillator at time $t = \text{poly}(q)$ is either $\Omega(1/\text{poly}(q))$ (yes instance) or $O(1/\exp(\sqrt{q}))$ (no instance).

## Caveat

The $|-\rangle$ resource state must be part of the initial condition. For the oscillator interpretation, this means $\dot{x}_1(0) = +1$, $\dot{x}_2(0) = -1$ (two oscillators encoding the $|-\rangle$ state in their velocity difference). Requires all Hadamard gates to act on the same qubit (achievable via SWAP gates from the $\{X, \text{Toff}\}$ gate set), which may increase circuit depth.

## Related notes
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]]
- [[Glued Trees as Coupled Oscillators]]
