# Extended Observable Trick for Nonunitary Preconditioners

> **Source:** Bagherimehrab, Nakaji, Wiebe, Brennen, Sanders & Aspuru-Guzik, arXiv:2306.11802
> **Tags:** #trick #QLSA #preconditioning #observables #LCU #postselection

## What it does

Avoids the postselection cost of applying a badly conditioned non-unitary preconditioner by decomposing it into unitaries and estimating the target observable in an ancilla-extended space.

## The trick

Suppose the desired solution state has the form

$$
|u\rangle=W^\dagger P A_P^{-1} P W|b\rangle,
$$

where $P$ is a non-unitary preconditioner. Directly block-encoding and postselecting $P$ can be expensive: in the wavelet PDE case, $\lambda_{\min}(P)=2/N$, so direct postselection followed by amplitude amplification costs $\Omega(N)$.

If $0\le P\le I$ is diagonal or otherwise easy to functional-calculus, write

$$
P=\frac{U^++U^-}{2},\qquad
U^\pm=P\pm i\sqrt{I-P^2}=e^{\pm i\arccos P}.
$$

Then define

$$
|\psi_{ab}\rangle=W^\dagger U^a A_P^{-1}U^b W|b\rangle,
\qquad a,b\in\{0,1\},
$$

where $U^{0/1}\equiv U^{+/-}$. Since

$$
|u\rangle=\frac{1}{4}\sum_{a,b}|\psi_{ab}\rangle,
$$

prepare instead

$$
|\psi\rangle=\frac{1}{2\xi}\sum_{a,b}|ab\rangle|\psi_{ab}\rangle,
\qquad
\xi^2=\frac{1}{4}\sum_{a,b}\lVert |\psi_{ab}\rangle\rVert^2.
$$

For an observable $M$ on the original solution space, define

$$
M'=\sum_{a,b,c,d}|ab\rangle\langle cd|\otimes M.
$$

Then

$$
\langle \psi|M'|\psi\rangle=\frac{4}{\xi^2}\langle u|M|u\rangle.
$$

So the non-unitary $P$ is never postselected as a map on the state. Its effect is moved into coherent branches and reconstructed at the observable level.

## When to reach for it

- Preconditioned [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] algorithms where the preconditioner is easy to express as a sum of a few unitaries but expensive to apply by postselection.
- Problems where only expectation values or local observables are needed, not full state preparation.
- Any setting where a non-unitary scaling map has large condition number but simple spectral structure.

## Complexity

In the source paper, $M'$ is a $4\times4$ block matrix with $M$ in every block, so sparse-access oracles for $M'$ use a single query to the corresponding oracles for $M$. The coherent branches require two ancilla qubits and controlled implementations of $U^\pm$ around the inverse block-encoding of $A_P$.

## Caveat

You pay in interpretation: the algorithm estimates $\langle u|M|u\rangle$ through $\langle\psi|M'|\psi\rangle$ and a normalisation factor $\xi^2$. If the task needs samples from $|u\rangle$ itself, or full classical readout of $u$, this trick does not remove that bottleneck.

The trick also depends on $P$ having a cheap unitary decomposition. A dense arbitrary preconditioner will not magically become easy.

## Related notes

- [[Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023) — Paper Notes]]
- [[Wavelet Preconditioning for Quantum PDE Solvers]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Near-Unitary Pairing for High-Success LCU]]
- [[Measurement Extraction Bottleneck for QLSA Applications]]
- [[Term-by-Term Expectation Estimation]]
