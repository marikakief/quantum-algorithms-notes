# Average-Case vs Worst-Case QML Advantage Test

> **Source:** Huang, Kueng, Preskill, arXiv:2101.02464
> **Tags:** #trick #quantum-ml #quantum-advantage #learning-theory

## What it does

Separates QML advantage claims into average-case prediction claims, where exponential query advantage is heavily constrained, and worst-case property-prediction claims, where quantum memory can give exponential separations.

## The trick

Before analysing a claimed QML advantage, ask which error criterion is being used.

**Average-case prediction:**

$$\mathbb E_{x\sim D}|h(x)-f(x)|^2\le \varepsilon.$$

For this setting, Huang--Kueng--Preskill show that if a quantum learner uses $N_Q$ queries to a quantum process $\mathcal E$, then a restricted classical learner can match the prediction error up to constants using

$$N_C=O(mN_Q/\varepsilon)$$

queries, where $m$ is the number of output qubits measured by the target observable.

**Worst-case property prediction:**

$$|\hat f_i-f_i|\le \varepsilon\qquad\text{for all }i.$$

For incompatible observables, quantum memory can matter enormously. The same paper gives a task where a quantum learner predicts all $4^n$ Pauli expectations using $O(n)$ copies for constant error, while any classical learner needs $2^{\Omega(n)}$ copies.

## When to reach for it

Use this as a sanity check for QML papers:

- If the claim is about typical prediction from data, be skeptical of exponential query advantage unless the model uses extra computational assumptions.
- If the claim is about worst-case prediction of many incompatible observables, quantum-memory separations are plausible.
- If the paper silently shifts between these two settings, that is a red flag.

## Complexity

The average-case simulation overhead is polynomial in $m$, $N_Q$, and $1/\varepsilon$. The worst-case Pauli separation is exponential: $O(n)$ quantum copies versus $2^{\Omega(n)}$ classical copies for all Paulis at constant accuracy.

## Caveat

This test is about query/sample complexity, not full computational runtime. A classical learner with polynomially many samples may still be computationally infeasible if the hypothesis class or empirical-risk problem is hard.

## Related notes

- [[Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) — Paper Notes]]
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]
- [[Bell Difference Sampling for Magnitude Estimation]]
- [[Holevo Information Bottleneck for Learning Lower Bounds]]
