# Biased Pauli Analysis for Product-State Learning

> **Source:** Chen, de Dios Pont, Hsieh, Huang, Lange, Li, arXiv:2409.03684
> **Tags:** #trick #process-learning #biased-fourier #product-distributions #heisenberg-picture

## What it does

Extends low-degree Pauli learning from locally flat input distributions to biased product distributions by replacing the ordinary Pauli basis with a distribution-adapted centered basis.

## The trick

When the one-qubit input distribution $D$ has nonzero bias, ordinary Pauli coordinates are no longer mean-zero under $D$, so the HCP23 truncation argument breaks. Fix this by centering the local operator basis around the marginal mean. After rotating to the bias axis, define

$$
\widetilde Z = \frac{Z-\mu I}{\sqrt{1-\mu^2}}, \qquad \mu = \mathbb E_{\rho\sim D}[\operatorname{Tr}(Z\rho)].
$$

Together with matching transverse directions, tensor these centered one-qubit operators to form a biased Pauli basis on $n$ qubits. Expand the Heisenberg-evolved observable in this basis and truncate by biased degree rather than ordinary Pauli weight.

This restores the right mean-zero structure for low-degree regression even though trace orthogonality is lost.

## When to reach for it

- Product-state inputs are biased rather than locally flat.
- Standard Pauli/Fourier truncation fails because the coordinates have nonzero mean.
- You need a distribution-adapted low-degree basis.

## Complexity

The trick changes the feature basis, not the outer learning loop. Runtime is driven by the number of retained biased-basis monomials up to the chosen degree.

## Caveat

This relies heavily on product structure. For correlated input distributions there is no comparable clean tensor-basis story.

## Related notes

- [[Predicting quantum channels over general product distributions (Chen-de Dios Pont-Hsieh-Huang-Lange-Li 2024) — Paper Notes]]
- [[Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) — Paper Notes]]
- [[Thresholded Low-Degree Heisenberg Learning]]
