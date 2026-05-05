# Quadratic-Form Comparison for Biased Low-Degree Decay

> **Source:** Chen, de Dios Pont, Hsieh, Huang, Lange, Li, arXiv:2409.03684
> **Tags:** #trick #biased-fourier #matrix-inequalities #process-learning

## What it does

Proves that high-degree components of a biased-basis expansion decay fast enough for learning by comparing the prediction-error quadratic form to a bounded reference quadratic form.

## The trick

In the biased basis, the natural distribution inner product is no longer aligned with the trace inner product, so coefficient truncation cannot be analyzed by the usual orthogonality shortcut. Instead write both the hard quantity and an easy bounded quantity as quadratic forms in the coefficient vector of the high-degree tail:

$$
\text{error} = v^\top M' v, \qquad \text{reference} = v^\top M v.
$$

Then prove a matrix inequality of the form

$$
v^\top M' v \le (1-\eta') \, v^\top \operatorname{Re}(M) v.
$$

Because the reference quadratic form is state-independently bounded, this forces geometric decay of the high-degree tail under repeated degree truncation.

## When to reach for it

- The basis you want is adapted to a distribution rather than to the trace inner product.
- Ordinary coefficient orthogonality is gone.
- You can express both the target error and a bounded comparison quantity as quadratic forms.

## Complexity

This is an analysis trick, not an algorithmic subroutine. Its computational cost is whatever is needed to estimate or control the resulting low-degree model.

## Caveat

You need a nontrivial comparison matrix inequality. Without that structural step, the trick is just notation.

## Related notes

- [[Predicting quantum channels over general product distributions (Chen-de Dios Pont-Hsieh-Huang-Lange-Li 2024) — Paper Notes]]
- [[Biased Pauli Analysis for Product-State Learning]]
- [[Quantum Bohnenblust--Hille Inequality for Local Observables]]
