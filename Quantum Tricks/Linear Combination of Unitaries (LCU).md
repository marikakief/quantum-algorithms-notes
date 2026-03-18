# Linear Combination of Unitaries (LCU)

> **Tags:** #trick #LCU #block-encoding #fundamental
> **Source:** Berry, Childs, Cleve, Kothari, Somma. arXiv:1412.4687 (clean PREPARE/SELECT formulation); LCU primitive idea originates in Childs–Wiebe arXiv:1202.5822

## What it does
Implements a non-unitary operator $\tilde{U} = \sum_j \beta_j V_j$ ([[Linear Combination of Unitaries (LCU)|linear combination of unitaries]]) as a probabilistic quantum operation, with success heralded by ancilla measurement.

## The trick
Two oracles:
1. **[[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]]** $B$: $B|0\rangle = \frac{1}{\sqrt{s}} \sum_j \sqrt{\beta_j}|j\rangle$ where $s = \sum_j \beta_j$
2. **[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]**: $\text{[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]}(V)|j\rangle|\psi\rangle = |j\rangle V_j|\psi\rangle$

Compose as $W = (B^\dagger \otimes \mathbb{1}) \cdot \text{[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]}(V) \cdot (B \otimes \mathbb{1})$.

Then: $W|0\rangle|\psi\rangle = \frac{1}{s}|0\rangle\tilde{U}|\psi\rangle + |0^\perp\rangle$

Projecting ancilla onto $|0\rangle$ yields $\tilde{U}|\psi\rangle / s$. Success probability $\sim 1/s^2$.

## When to reach for it
- **The** fundamental primitive for [[Block-Encoding Composition Algebra|block-encoding construction]]
- Hamiltonian simulation via Taylor series
- Any non-unitary operator expressible as weighted sum of unitaries
- Building block for [[QSVT Meta-Template|QSVT]], [[Qubitization Iterate|qubitisation]], and essentially all modern quantum algorithms

## Complexity
One use of [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] ($O(L)$ or $O(\log L)$ gates depending on structure) + one use of [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] ($O(L)$ controlled unitaries). Total normalisation factor $s = \sum |\beta_j|$ determines [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] rescaling.

## Caveat
Success probability $1/s^2$ can be small. Use [[Oblivious Amplitude Amplification (Robust)|oblivious amplitude amplification]] to boost deterministically when $s$ is known.

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — clean PREPARE/SELECT formulation; the standard reference
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
