# Quantum-Accelerated Multilevel Monte Carlo Methods for Stochastic Differential Equations in Mathematical Finance (An-Linden-Liu-Montanaro-Shao-Wang 2021) — Paper Notes

> **Source:** Dong An, Noah Linden, Jin-Peng Liu, Ashley Montanaro, Changpeng Shao, and Jiasu Wang, *Quantum-accelerated multilevel Monte Carlo methods for stochastic differential equations in mathematical finance*, arXiv:2012.06283, Quantum **5**, 481 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2012.06283) · [Quantum](https://doi.org/10.22331/q-2021-06-24-481)
> **Tags:** #quantum-finance #monte-carlo #multilevel-monte-carlo #SDE #amplitude-estimation #option-pricing #Feynman-Kac

---

## The computational problem

The paper studies scalar-output estimation for payoffs defined by a stochastic differential equation

$$
dX_t = \mu(X_t,t)\,dt + \sigma(X_t,t)\,dW_t, \qquad t \in [0,T].
$$

**Input model:** the algorithm is given classical sampling access and coherent quantum access to:

- $O_I$: samples the initial condition $X_0 \sim \pi_0$;
- $O_W$: samples Brownian increments;
- $O_\mu$ and $O_\sigma$: evaluate the drift and volatility functions;
- $O_P$: evaluates the payoff functional $P$.

The quantum version assumes these sampling/evaluation procedures can be run reversibly, so a classical sampler $A$ for the discretised path can be promoted to a unitary subroutine and used inside [[Quantum Mean Estimation via Amplitude Estimation]]. For payoff evaluation they write, for example,

$$
U_P |x\rangle|0\rangle = |x\rangle|P(x)\rangle.
$$

**Output:** estimate

$$
\mathbb{E}[P(X_T)] \quad \text{with } X_0 \sim \pi_0
$$

to additive error $\epsilon$ with success probability at least $0.99$.

The motivating cases are option prices and Greeks in mathematical finance, where $X_t$ may be a Black--Scholes or local-volatility process and $P$ is a discounted payoff.

---

## What the paper does

This is the multilevel analogue of [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro's quantum Monte Carlo speedup]]. Classical [[Multilevel Monte Carlo]] reduces the discretisation cost of SDE simulation, but still has the usual $\epsilon^{-2}$ sampling wall. This paper applies quantum mean estimation separately to each level of the MLMC telescoping sum and proves that the quadratic precision speedup survives the level balancing.

The headline bound: for general piecewise-Lipschitz payoffs, QA-MLMC reaches

$$
\widetilde{O}(\epsilon^{-1})
$$

provided the SDE discretisation scheme has strong order $r>2$. For globally Lipschitz payoffs, the requirement drops to $r\ge 1$ (Milstein suffices). My assessment: the algorithmic move is clean and useful; the main caveat is the strong-order requirement, especially for discontinuous payoffs such as digital options.

---

## The algorithm / construction

### 1. Classical SDE discretisation

Choose a numerical SDE scheme with step size $h=T/n$. For the Milstein scheme,

$$
\widehat X_{k+1}
= \widehat X_k + \mu(\widehat X_k,t_k)h
+ \sigma(\widehat X_k,t_k)\Delta W_k
+ \frac12\sigma(\widehat X_k,t_k)\partial_X\sigma(\widehat X_k,t_k)
\big((\Delta W_k)^2-h\big).
$$

More generally, a strong-order-$r$ scheme satisfies

$$
\mathbb{E}\!
\left[\sup_{0\le kh\le T}|\widehat X_k-X_{kh}|^m\right]
\le C_m h^{rm}.
$$

A direct Monte Carlo estimator using this discretisation has two errors: sampling error $O(N^{-1/2})$ and discretisation bias $O(h^r)$. Classically this costs $O(\epsilon^{-2-1/r})$; quantum-accelerated single-level Monte Carlo costs $\widetilde{O}(\epsilon^{-1-1/r})$.

### 2. Multilevel telescoping

Define level-$\ell$ approximations $P_\ell=P(\widehat X_{n_\ell})$ with $n_\ell=2^\ell$. MLMC uses

$$
\mathbb{E}[P_L]
= \mathbb{E}[P_0] + \sum_{\ell=1}^{L}\mathbb{E}[P_\ell-P_{\ell-1}],
$$

where $P_\ell$ and $P_{\ell-1}$ are evaluated on coupled Brownian paths. The coupling is the point: high levels are expensive but the variance of $P_\ell-P_{\ell-1}$ is small.

Write:

$$
C_\ell = O(2^{\gamma \ell}), \qquad
V_\ell = \operatorname{Var}(P_\ell-P_{\ell-1}) = O(2^{-\beta \ell}),
$$

and suppose the bias satisfies

$$
|\mathbb{E}[P_\ell-P]| = O(2^{-\alpha \ell}).
$$

Here $\alpha$ is the weak/bias convergence exponent, $\beta$ is the variance-decay exponent for level differences, and $\gamma$ is the cost-growth exponent per level. In the quantum theorem, $\widehat{\beta}=\beta/2$ because mean estimation gains a square root in the variance dependence.

### 3. Quantum acceleration level by level

For each level, use [[Quantum Mean Estimation via Amplitude Estimation]] to estimate

$$
\mathbb{E}[P_\ell-P_{\ell-1}]
$$

to tolerance $\epsilon_\ell$. If $\widehat\beta=\beta/2$, quantum mean estimation uses roughly $2^{-\widehat\beta \ell}/\epsilon_\ell$ level calls rather than $2^{-\beta \ell}/\epsilon_\ell^2$ classical samples, after normalizing the payoff range/variance information for amplitude estimation.

The error allocation is chosen so that

$$
\sum_{\ell=0}^L \epsilon_\ell \le \epsilon/2,
$$

while the discretisation level $L$ is chosen so that the bias is at most $\epsilon/2$:

$$
L = \left\lceil \frac{\log(2/\epsilon)}{\alpha} \right\rceil.
$$

This gives Theorem 2's general QA-MLMC cost:

$$
\begin{cases}
O\!\left(\epsilon^{-1}(\log(1/\epsilon))^{3/2}(\log\log(1/\epsilon))^2\right), & \widehat\beta>\gamma,\\[2mm]
O\!\left(\epsilon^{-1}(\log(1/\epsilon))^{7/2}(\log\log(1/\epsilon))^2\right), & \widehat\beta=\gamma,\\[2mm]
O\!\left(\epsilon^{-1-(\gamma-\widehat\beta)/\alpha}(\log(1/\epsilon))^{3/2}(\log\log(1/\epsilon))^2\right), & \widehat\beta<\gamma.
\end{cases}
$$

### 4. Plug in SDE convergence parameters

For SDE path simulation, the cost doubles with the level, so $\gamma=1$.

For piecewise-Lipschitz payoffs, the paper proves

$$
\alpha = r-o(1), \qquad \beta = r-o(1), \qquad \gamma=1.
$$

The $o(1)$ comes from bounding the probability that the numerical path crosses a payoff discontinuity. For globally Lipschitz payoffs, there is no jump-crossing term, and the parameters improve to

$$
\alpha=r, \qquad \beta=2r, \qquad \gamma=1.
$$

---

## Key results

### Single-level quantum Monte Carlo for SDEs

If a strong-order-$r$ discretisation produces $\widehat X_n$ and $P(\widehat X_n)$ has bounded variance independent of $h$, then quantum-accelerated Monte Carlo estimates $\mathbb{E}[P(X_T)]$ to additive error $\epsilon$ with cost

$$
\widetilde{O}(\epsilon^{-1-1/r}).
$$

This is the direct application of [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro 2015]]; MLMC improves the discretisation overhead.

### General QA-MLMC theorem

For a level hierarchy with parameters $(\alpha,\beta,\gamma)$ and $\widehat\beta=\beta/2$, QA-MLMC has the three-regime cost above. The threshold is $\widehat\beta$ versus $\gamma$: variance decay must beat level cost growth for the clean $\widetilde O(\epsilon^{-1})$ scaling.

### SDE payoff theorem

For SDEs satisfying the paper's Lipschitz and moment assumptions, and for piecewise-Lipschitz payoffs,

The boundary case around $r=2$ is absorbed into the displayed logarithmic and $o(1)$ factors in this summary.

$$
\text{QA-MLMC cost}=
\begin{cases}
O\!\left(\epsilon^{-1}(\log(1/\epsilon))^{3/2}(\log\log(1/\epsilon))^2\right), & r>2,\\[2mm]
O\!\left(\epsilon^{-1/2-1/r-o(1)}(\log(1/\epsilon))^{3/2}(\log\log(1/\epsilon))^2\right), & r\le 2.
\end{cases}
$$

For globally Lipschitz payoffs,

$$
\text{QA-MLMC cost}=
\begin{cases}
O\!\left(\epsilon^{-1}(\log(1/\epsilon))^{3/2}(\log\log(1/\epsilon))^2\right), & r>1,\\[2mm]
O\!\left(\epsilon^{-1}(\log(1/\epsilon))^{7/2}(\log\log(1/\epsilon))^2\right), & r=1,\\[2mm]
O\!\left(\epsilon^{-1/r}(\log(1/\epsilon))^{3/2}(\log\log(1/\epsilon))^2\right), & r<1.
\end{cases}
$$

So Milstein ($r=1$) is enough for near-linear precision scaling when the payoff is globally Lipschitz; discontinuous payoffs need higher-order schemes.

### Applications

- **Black--Scholes payoffs:** recovers $\widetilde O(\epsilon^{-1})$ for European/Asian-style Lipschitz payoffs; digital payoffs need strong order $r>2$ for the clean bound.
- **Local volatility model:** gives the same QA-MLMC bounds for models without closed-form path solutions.
- **Greeks:** uses Malliavin-calculus representations, e.g. Delta as

$$
\frac{\partial u(s,0)}{\partial s}
= \mathbb{E}\!\left[
\frac{1}{T} P(X_T)\int_0^T Y_t\sigma^{-1}(X_t)\,dW_t
\mid X_0=s,Y_0=1
\right],
$$

where $Y_t$ satisfies

$$
dY_t = \mu'(X_t)Y_t\,dt + \sigma'(X_t)Y_t\,dW_t.
$$

This puts Greeks into the same expectation-estimation format.

- **Binomial option pricing:** for piecewise-constant payoffs, sublinear binomial sampling plus quantum mean estimation gives $\widetilde O(\epsilon^{-1})$.

---

## Comparison with prior work

| Method | Setting | Cost in $\epsilon$ | Comment |
|---|---|---:|---|
| Classical MC with strong-order-$r$ scheme | general SDE payoff | $O(\epsilon^{-2-1/r})$ | sampling and discretisation both paid directly |
| Quantum MC with strong-order-$r$ scheme | general SDE payoff | $\widetilde O(\epsilon^{-1-1/r})$ | direct [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro 2015]] application |
| Classical MLMC | strong-order $r>1$ piecewise-Lipschitz payoff | $O(\epsilon^{-2})$ | removes most discretisation overhead |
| QA-MLMC | strong-order $r>2$ piecewise-Lipschitz payoff | $\widetilde O(\epsilon^{-1})$ | quantum speedup survives level balancing |
| QA-MLMC | globally Lipschitz payoff, $r\ge 1$ | $\widetilde O(\epsilon^{-1})$ | Milstein is enough |
| [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes|Rebentrost-Gupt-Bromley 2018]] | explicit Black--Scholes sampling | $\widetilde O(\epsilon^{-1})$ | does not handle general SDEs without closed form |

---

## Limits / caveats

- **Strong-order requirement.** For discontinuous or merely piecewise-Lipschitz payoffs, the clean $\widetilde O(\epsilon^{-1})$ bound requires $r>2$. High strong-order schemes are mathematically standard but not cheap to implement, especially in higher-dimensional SDEs.
- **Coherent sampling assumption.** The quantum algorithm needs reversible implementations of Brownian increment sampling and of the numerical path simulator. The paper treats oracle costs as $O(1)$; arithmetic, random-bit loading, and fixed-point precision are hidden.
- **Scalar output only.** The algorithm estimates an expectation. It does not output a path distribution or a quantum state representing the SDE solution.
- **Quadratic precision speedup, not more.** This is the same $1/\epsilon^2 \to 1/\epsilon$ story as other quantum Monte Carlo algorithms. It matters asymptotically but will be expensive fault-tolerantly.
- **Finance examples are one-dimensional.** The paper says the analysis extends to higher-dimensional systems under separable jump-discontinuity assumptions, but the detailed examples are one-dimensional option-pricing models.
- **Classical competitors.** Quasi-Monte Carlo, variance reduction, analytic formulas, and problem-specific PDE solvers can beat plain MC/MLMC on structured finance problems. The right comparison is problem-specific, not generic MC.

---

## Reusable ideas

1. [[Quantum-Accelerated MLMC Error Allocation]] — estimate each term in an MLMC telescoping sum with quantum mean estimation, choosing per-level error tolerances so the total additive error is $O(\epsilon)$ while preserving the variance/cost balance.

2. [[Jump-Crossing Bound for Discontinuous Payoffs]] — control piecewise-Lipschitz payoff error by splitting numerical paths according to whether they cross a payoff discontinuity. This explains why discontinuous digital-style payoffs need higher strong order.

3. [[Path-Dependent Payoffs by State Augmentation]] — convert path-dependent payoffs involving $\int f(X_t)dt$ or $\int g(X_t)dW_t$ into terminal payoffs of an augmented SDE.

4. [[Malliavin Greek Estimation as a Payoff Functional]] — express Greeks as expectations of payoff-weighted stochastic integrals, putting sensitivity estimation into the same QA-MLMC pipeline.

5. [[Sublinear Binomial Sampling for Quantum Option Pricing]] — use fast binomial sampling and threshold payoff evaluation so a binomial lattice model can be sampled in polylogarithmic time per path, then apply quantum mean estimation.

---

## References within this paper

- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — the mean-estimation primitive used at every MLMC level.
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes|Rebentrost-Gupt-Bromley (2018)]] — previous quantum finance pricing via explicit Black--Scholes sampling.
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes|Linden-Montanaro-Shao (2022)]] — related use of fast random/binomial sampling plus amplitude estimation; this SDE paper cites the preprint version.
- Giles (2008, 2015) — classical MLMC for SDE path simulation.
- Giles, Higham, Mao (2009) — MLMC analysis for options with non-globally-Lipschitz payoffs.
- Kloeden-Platen (2013) — numerical SDE schemes and strong convergence.
- Fournié et al. (1999) — Malliavin calculus for Greeks.
- Black-Scholes (1973), Cox-Ross-Rubinstein (1979), Jarrow-Rudd (1983) — finance models used as applications.
- Bringmann et al. (2014), Farach-Colton-Tsai (2015) — sublinear binomial sampling.

---

## Cross-links

### Paper notes

- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]]
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]]
- [[Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023) — Paper Notes]]

### Trick cards

- [[Multilevel Monte Carlo]]
- [[Quantum-Accelerated MLMC Error Allocation]]
- [[Jump-Crossing Bound for Discontinuous Payoffs]]
- [[Path-Dependent Payoffs by State Augmentation]]
- [[Malliavin Greek Estimation as a Payoff Functional]]
- [[Sublinear Binomial Sampling for Quantum Option Pricing]]
- [[Quantum Mean Estimation via Amplitude Estimation]]
- [[Payoff Encoding via Ancilla Rotation]]
- [[Amplitude Estimation Applied to Random Walk PDE Solvers]]
