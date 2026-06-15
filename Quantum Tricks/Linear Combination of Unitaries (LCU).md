> **Source:** Andrew M. Childs and Nathan Wiebe, arXiv:1202.5822 (first coherent LCU construction for [[Hamiltonian simulation]]); Berry, Childs, Cleve, Kothari, and Somma, arXiv:1412.4687 for the clean PREPARE/SELECT formulation
> **Tags:** #trick #LCU #block-encoding #fundamental

## What it does
Implements a non-unitary operator $\tilde{U} = \sum_j \beta_j V_j$ ([[Linear Combination of Unitaries (LCU)|linear combination of unitaries]]) as a probabilistic quantum operation, with success heralded by ancilla measurement.

## The trick
Two oracles:
1. **[[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]]** $B$: for signed or complex coefficients, write $\beta_j = |\beta_j|e^{i\phi_j}$ and prepare
   $$
   B|0\rangle = \frac{1}{\sqrt{s}} \sum_j \sqrt{|\beta_j|}|j\rangle,\qquad s=\sum_j|\beta_j|.
   $$
2. **[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]**: apply the coefficient phases in the controlled branches,
   $$
   \mathrm{SELECT}|j\rangle|\psi\rangle = |j\rangle e^{i\phi_j}V_j|\psi\rangle.
   $$

Compose as $W = (B^\dagger \otimes \mathbb{1}) \cdot \text{[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]}(V) \cdot (B \otimes \mathbb{1})$.

Then:
$$
W|0\rangle|\psi\rangle = \frac{1}{s}|0\rangle\tilde{U}|\psi\rangle + |\Phi^\perp\rangle,
$$
where $|\Phi^\perp\rangle$ has ancilla support orthogonal to $|0\rangle$ but may remain entangled with the system.

Projecting ancilla onto $|0\rangle$ yields $\tilde{U}|\psi\rangle / s$. Success probability is

$$
\frac{\|\tilde{U}|\psi\rangle\|^2}{s^2},
$$

which is exactly $1/s^2$ only when $\tilde{U}$ is unitary on the input state.

## When to reach for it
- **The** fundamental primitive for [[Block-Encoding Composition Algebra|block-encoding construction]]
- [[Hamiltonian simulation]] via Taylor series
- Any non-unitary operator expressible as weighted sum of unitaries
- Building block for many [[QSVT Meta-Template|QSVT]], [[Qubitization Iterate|qubitisation]], and block-encoding constructions

## Complexity
One use of [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] ($O(L)$ or $O(\log L)$ gates depending on structure) + one use of [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] ($O(L)$ controlled unitaries). Total normalisation factor $s = \sum_j |\beta_j|$ determines the block-encoding rescaling: this is an \((s,a,0)\)-block-encoding of $\tilde{U}$, where $a$ is the number of ancilla qubits used by PREPARE.

## Caveat
Success probability can be small. Use [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]] to boost deterministically only in the near-unitary/uniform-success setting required by OAA; for a general nonunitary contraction, QSVT or other block-encoding postselection-removal tools may be the right abstraction instead.

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — clean PREPARE/SELECT formulation; the standard reference
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
