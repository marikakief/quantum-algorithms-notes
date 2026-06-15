# Review: Quantum Computational Finance - Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018)

Source note: [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]]

## Verdict

Minor issues. The note is appropriately cautious and already includes the important Herbert caveat. The main refinements are to keep the result framed as a query/mean-estimation speedup and to avoid hiding arithmetic costs.

## Comments

- The `tilde O(lambda/epsilon)` cost is a query-level amplitude-estimation statement. It should always be paired with the cost of preparing the distribution and reversibly computing the payoff.

- For Black-Scholes, Gaussian CDF access is analytically plausible, so Grover-Rudolph state preparation is less suspect than in generic log-concave settings. The note says this; it should be in the main caveat, not just the final paragraph.

- The payoff rotation requires reversible exponential, max/comparator, normalization, and arcsine/rotation synthesis. These are not small constants in a fault-tolerant implementation.

- The variance parameter `lambda` should be defined consistently. It is not a universal finance risk measure; it is the standard-deviation/range parameter entering the Monte Carlo mean-estimation bound.

- The Asian-option extension should mention correlations between Brownian increments if the time-grid model is not phrased as independent increments.

## Suggested Additions

- Add an end-to-end resource accounting box: distribution loading, payoff arithmetic, amplitude-estimation iterations, and final confidence boosting.
- Add a classical-baseline note for quasi-Monte Carlo, variance reduction, and multilevel Monte Carlo.
