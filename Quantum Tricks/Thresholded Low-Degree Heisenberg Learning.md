# Thresholded Low-Degree Heisenberg Learning

> **Source:** Huang, Chen, Preskill, arXiv:2210.14894
> **Tags:** #trick #heisenberg-picture #process-learning #classical-shadows

## What it does

Learns predictions for a complicated quantum process by estimating only the significant low-weight Pauli coefficients of the Heisenberg-evolved observable.

## The trick

Rewrite the target quantity as

$$
\operatorname{tr}(O\,\mathcal E(\rho)) = \operatorname{tr}(\mathcal E^\dagger(O)\rho).
$$

Now expand $\mathcal E^\dagger(O)$ in the Pauli basis and truncate to degree $k$:

$$
\mathcal E^\dagger(O) \approx \sum_{|P|\le k} \alpha_P P.
$$

Estimate the low-weight coefficients from randomized product-state inputs and local classical-shadow outputs, then apply a threshold rule: keep only coefficients whose empirical magnitude is large enough to beat the noise floor. The predictor is

$$
h(\rho,O)=\sum_{|P|\le k} \hat\alpha_P\operatorname{tr}(P\rho).
$$

This works when the input-state distribution kills high-degree terms on average, so low-degree truncation captures most of the signal.

## When to reach for it

- The process is too complicated to learn globally, but you only need local output properties.
- The input distribution has enough symmetry or randomness that high-degree terms average away.
- You can estimate low-weight Heisenberg coefficients cheaply.

## Complexity

The learned model has size roughly the number of retained Pauli strings up to degree $k=O(\log(1/\varepsilon))$. Sample complexity is polylogarithmic in system size at constant precision, but quasi-polynomial in $1/\varepsilon$.

## Caveat

If the input distribution is not locally flat enough, or if the Heisenberg-evolved observable spreads substantial weight into many low-degree coefficients, the thresholding argument weakens fast.

## Related notes

- [[Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) — Paper Notes]]
- [[Predicting quantum channels over general product distributions (Chen-de Dios Pont-Hsieh-Huang-Lange-Li 2024) — Paper Notes]]
- [[Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) — Paper Notes]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Quantum Bohnenblust--Hille Inequality for Local Observables]]
