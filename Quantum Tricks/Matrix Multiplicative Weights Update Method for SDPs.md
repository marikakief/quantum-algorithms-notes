# Matrix Multiplicative Weights Update Method for SDPs

> **Source:** Arora, Hazan & Kale (2005); Kale (2007); applied to quantum complexity by Jain, Ji, Upadhyay & Watrous, arXiv:0907.4737
> **Tags:** #trick #SDP #optimization #multiplicative-weights #complexity #PSPACE

## What it does
Solves semidefinite programs iteratively using matrix exponential updates, in a way that can be parallelised to logarithmic depth — making it suitable for space-bounded computation.

## The trick
The classical multiplicative weights update maintains scalar weights $w_i$ and updates them as $w_i \leftarrow w_i \cdot e^{-\eta \ell_i}$ based on a loss vector $\ell$. The matrix version replaces scalars with density matrices and scalar exponentials with matrix exponentials.

Given an SDP with primal variable $X \in \mathrm{Pos}(\mathcal{V})$ and constraints involving a map $\Phi$:

1. Initialise $W_0 = \mathbf{1}$, $\rho_0 = W_0/\mathrm{Tr}(W_0)$.
2. At each step $t$, an oracle computes a "constraint violation" operator $\Pi_t$ (typically a projection onto the positive eigenspace of some residual).
3. Update:
$$W_{t+1} = \exp\left(-\varepsilon\delta \sum_{j=0}^{t} \Phi^*(\Pi_j / \beta_j)\right), \quad \rho_{t+1} = W_{t+1}/\mathrm{Tr}(W_{t+1})$$

The convergence relies on the Golden-Thompson inequality $\mathrm{Tr}[\exp(A+B)] \leq \mathrm{Tr}[\exp(A)\exp(B)]$ and matrix exponential sandwich bounds: for $0 \leq P \leq \mathbf{1}$,
$$\exp(-\eta P) \leq \mathbf{1} - \eta e^{-\eta} P$$

After $T = O(\log N / \varepsilon^3)$ iterations, the algorithm either finds an approximate primal feasible solution (if it "accepts" early) or constructs an approximate dual feasible solution (if all iterations complete), establishing the SDP value to within the promised gap.

The critical property for complexity theory: each iteration requires only a matrix exponential and a spectral decomposition, both computable in NC. Composing $O(\log N)$ NC-computable iterations remains in NC. So the full algorithm lives in NC(poly) = PSPACE.

## When to reach for it
- Solving SDPs where you need the solution to be computable in bounded space (PSPACE) or bounded depth (NC(poly)), rather than bounded time.
- When interior-point methods are too sequential — MMW parallelises while interior-point methods don't.
- Complexity-theoretic proofs that something is in PSPACE: if you can formulate the problem as an SDP with polynomially many "rounds" and NC-computable oracles, MMW gives you containment.
- The method has also been applied to quantum refereed games (QRG(1) $\subseteq$ PSPACE) and could potentially be used for other quantum complexity class containments.

## Complexity
$O(\log N / \varepsilon^3)$ iterations, where $N$ is the matrix dimension and $\varepsilon$ is the gap. Each iteration is in NC (polylogarithmic depth, polynomial space). Total: NC(poly) = PSPACE.

## Caveat
The number of iterations is logarithmic in $N$ but polynomial in $1/\varepsilon$, so the method needs a constant or inverse-polynomial promise gap on the SDP value. For SDPs with exponentially small gaps, the iteration count blows up. The method also requires that the constraint-violation oracle is NC-computable, which is specific to the structure of the SDP at hand.

## Related notes
- [[QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011) — Paper Notes]]
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
- [[QMAM Reduction to Single-Coin SDP]]
