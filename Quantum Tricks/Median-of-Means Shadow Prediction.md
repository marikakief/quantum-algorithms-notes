# Median-of-Means Shadow Prediction

> **Source:** Huang, Kueng, Preskill, arXiv:2002.08953
> **Tags:** #trick #classical-shadows #statistics #measurement

## What it does

Converts heavy-tailed single-shadow observable estimates into simultaneous high-probability estimates for many observables.

## The trick

Classical-shadow snapshots give unbiased estimates, but a single value

$$\operatorname{tr}(O\hat\rho)$$

can have a heavy tail. Instead of averaging all snapshots at once, split $NK$ snapshots into $K$ batches of size $N$. Average within each batch,

$$\hat\rho_{(k)}=\frac{1}{N}\sum_{j\in \text{batch }k}\hat\rho_j,$$

then estimate each observable by

$$\hat o_i=\operatorname{median}_{1\le k\le K}\operatorname{tr}(O_i\hat\rho_{(k)}).$$

For $M$ observables, choosing

$$K=2\log(2M/\delta)$$

and

$$N=\frac{34}{\varepsilon^2}\max_i\left\|O_i-\frac{\operatorname{tr}(O_i)}{2^n}\mathbb I\right\|_{\rm shadow}^2$$

gives

$$|\hat o_i-\operatorname{tr}(O_i\rho)|\le \varepsilon$$

for all $i$ with probability at least $1-\delta$.

## When to reach for it

Use it when estimates must be reliable uniformly over a large observable list, and the raw estimator only has a variance bound rather than sub-Gaussian tails.

## Complexity

The median only costs a logarithmic factor in the number of observables and inverse failure probability:

$$O\!\left(\frac{\|O\|_{\rm shadow}^2\log(M/\delta)}{\varepsilon^2}\right)$$

measurements up to constants.

## Caveat

Median-of-means improves concentration, not variance. If the chosen measurement ensemble has huge shadow norm for the observable family, this trick cannot rescue it.

## Related notes

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Classical Shadow Channel Inversion]]
- [[Shadow Norm as Measurement-Observable Match]]
