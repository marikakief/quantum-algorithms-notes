
> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748 (2024)
> **Tags:** #trick #unitary-synthesis #MPS #state-preparation #circuit-compilation #fault-tolerant

## What it does

Synthesises only $N_{\text{un}}/d$ columns of an $N_{\text{un}} \times N_{\text{un}}$ unitary, as needed when the input is restricted to a subspace. Reduces the cost by roughly half per reduction step compared to synthesising the full unitary.

## The trick

When only half the columns (rows $A, B$) of a $2k \times 2k$ unitary are needed, decompose via SVD and QR:

$$\begin{pmatrix} A & ? \\ B & ? \end{pmatrix} = \begin{pmatrix} U_1 & 0 \\ 0 & U_2 \end{pmatrix} \begin{pmatrix} D_1 & ? \\ D_2 & ? \end{pmatrix} \begin{pmatrix} V & 0 \\ 0 & V \end{pmatrix}$$

where $D_1, D_2$ are diagonal. Obtain $U_1, D_1, V$ from SVD of $A$; then $U_2, D_2$ from QR of $BV^\dagger$ (upper triangularity of the QR factor, combined with unitarity constraints, forces $D_2$ to be diagonal).

The quantum circuit:
1. **Initial unitary** $V$ on the first $n - 1$ qubits (dimension $k$) — in the MPS context, this merges with the previous step's operations at zero extra cost.
2. **Controlled qubit rotation** on one qubit, controlled by the $k$-dimensional register — applies the diagonal phases from $D_1, D_2$. Cost: one [[QROM (Quantum Read-Only Memory)|QROM]] lookup + rotation.
3. **Controlled unitaries** $U_1, U_2$ on the $k$-dimensional register, controlled by the qubit — uses only $N_{\text{un}}/2$ layers of [[Rectangular Multiport Unitary Synthesis|rectangular multiport synthesis]] (half the layers of a full unitary).

For general $d > 2$ (synthesising $k = N_{\text{un}}/d$ columns of an $N_{\text{un}}$-dimensional unitary): iterate. QR-decompose the bottom $(d-1)k$ rows to extract a $(d-1)k \times (d-1)k$ unitary (of which only $k$ columns matter) and a $k \times k$ block. This gives $d - 1$ applications of the $d = 2$ scheme.

**MPS application:** For MPS preparation, the initial unitary $V$ at each step can be merged with the final unitary from the previous MPS site. This telescoping eliminates the dominant cost of $V$ entirely, making the per-site cost essentially that of the controlled rotation + controlled subspace unitaries.

## When to reach for it

- MPS state preparation, where each site contributes a unitary on $d\chi$ dimensions but only $\chi$ columns are needed (input physical register is $|0\rangle$).
- Any quantum circuit where a unitary acts on a register that is partially initialised to a known state.
- State preparation where the target state is specified as columns of a unitary in a larger Hilbert space.

## Complexity

For the $d = 2$ case: roughly half the cost of full unitary synthesis on the same dimension, plus one QROM-controlled rotation. The cost increase with $d$ is linear ($d - 1$ applications of the $d = 2$ scheme).

## Caveat

The linear scaling in $d$ means LKS ($\sqrt{d/2}$ scaling) is better for $d \gtrsim 30$. For MPS with $d = 4$ (the standard chemistry case with $\{|\varnothing\rangle, |\uparrow\rangle, |\downarrow\rangle, |\uparrow\downarrow\rangle\}$), this method wins by $\sim 3.5\times$.

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[Rectangular Multiport Unitary Synthesis]]
- [[QROM (Quantum Read-Only Memory)]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
