
> **Tags:** #trick #QSP #eigenphase #interface
> **Source:** Low–Chuang PRL 2017 (arXiv:1606.02685)

## What it does
Encodes an eigenphase of a large unitary into a controllable single-qubit rotation on an ancilla.

## The trick
Build
$$U_0 = |+\rangle\langle+|\otimes I + |-\rangle\langle-|\otimes W$$
so on eigenstate $|u_\lambda\rangle$ of $W$, ancilla undergoes a rotation parameterized by eigenphase $\theta_\lambda$.

Add single-qubit phase shifts to get phased iterate $U_\phi$.

This is the transducer from high-dimensional spectral data to one-qubit signal processing.

Explicitly, if \(W|u\rangle=e^{i\theta}|u\rangle\), then

$$
U_0|0\rangle|u\rangle
=\frac{|+\rangle+e^{i\theta}|-\rangle}{\sqrt2}|u\rangle
=e^{i\theta/2}\left(\cos\frac{\theta}{2}|0\rangle-i\sin\frac{\theta}{2}|1\rangle\right)|u\rangle
$$

up to the sign convention for \(|-\rangle\). Thus the eigenphase of \(W\) becomes a controllable single-qubit rotation angle. QSP then applies only the polynomial transformations allowed by its parity and boundedness constraints.

## When to use
Eigenphase/eigenvalue transform workflows where the signal unitary has the QSP-compatible controlled form and the desired scalar transformation has an admissible polynomial approximation.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
