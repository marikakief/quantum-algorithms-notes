> **Source:** Hsin-Yuan Huang, Sitan Chen, John Preskill, *Learning to predict arbitrary quantum processes*, arXiv:2210.14894, PRX Quantum **4**, 040337 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2210.14894) · [PRX Quantum](https://doi.org/10.1103/PRXQuantum.4.040337)
> **Tags:** #quantum-learning #process-learning #classical-shadows #heisenberg-picture #quantum-ml

---

## The computational problem

Given black-box access to an unknown $n$-qubit quantum process $\mathcal E$, learn a classical predictor for quantities of the form

$$
\operatorname{tr}\!igl(O\,\mathcal E(\rho)\bigr)
$$

where:

- $\rho$ is an input state drawn from a distribution $\mathcal D$ over $n$-qubit states,
- $O$ is a target observable, typically bounded-degree / few-body,
- the learner gets only a polynomial-size classical training set built from random product-state inputs and randomized single-qubit measurements on the outputs.

The real question is not tomography of $\mathcal E$ in full generality — that would still be exponential — but whether one can **predict local properties of the output** efficiently even when $\mathcal E$ itself may be an exponentially long quantum circuit or very long Hamiltonian evolution.

## What the paper does

This paper gives a clean yes, with caveats that matter. It shows that for a broad class of input distributions (their “locally flat” distributions) and bounded-degree observables, one can classically learn a predictor with small average error using only polylogarithmic dependence on system size and quasi-polynomial dependence on precision.

The conceptual move is simple and very good: push the observable backward,

$$
\operatorname{tr}\!\bigl(O\,\mathcal E(\rho)\bigr)=\operatorname{tr}\!\bigl(\mathcal E^\dagger(O)\,\rho\bigr),
$$

then learn a low-degree approximation to the Heisenberg-evolved observable $\mathcal E^\dagger(O)$ from classical-shadow-style data. The hard process disappears behind a learnable operator surrogate.

My assessment: this is one of Robert Huang’s strongest structural papers. [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes|Power of Data]] asked when data erases naive quantum advantage claims; this paper goes further and shows that even wildly complex quantum dynamics can become classically predictable for the right observables and input ensemble. The real engine is not “ML” in the vague sense; it is a sharp low-degree / low-influence statement plus a new quantum Bohnenblust--Hille-type norm inequality.

## The algorithm / construction

### 1. Training data from randomized experiments

For each sample $\ell=1,\dots,N$:

1. Prepare a random product stabilizer state $|\psi^{\mathrm{in}}_\ell\rangle$ with each qubit drawn from $
\{|0\rangle,|1\rangle,|+\rangle,| - \rangle,|y_+\rangle,|y_-\rangle\}$.
2. Apply the unknown process $\mathcal E$.
3. Measure each output qubit in a random Pauli basis, storing the resulting single-shot shadow state $|\psi^{\mathrm{out}}_\ell\rangle$.

This gives a classical dataset

$$
S_N(\mathcal E)=\{(|\psi^{\mathrm{in}}_\ell\rangle,|\psi^{\mathrm{out}}_\ell\rangle)\}_{\ell=1}^N.
$$

### 2. Convert process learning into observable learning

For a target observable $O$, define the Heisenberg-picture observable

$$
O' = \mathcal E^\dagger(O).
$$

Then predicting $\operatorname{tr}(O\mathcal E(\rho))$ is equivalent to predicting $\operatorname{tr}(O'\rho)$. The learner therefore estimates low-weight Pauli coefficients of $O'$.

### 3. Empirical low-weight Pauli estimates

For each Pauli string $P$ with support size $|P|\le k$, form an unbiased estimator of the coefficient of $P$ in $O'$:

$$
\hat p_P(O)
=
\frac1N\sum_{\ell=1}^N
\operatorname{tr}\!\bigl(P|\psi^{\mathrm{in}}_\ell\rangle\!\langle\psi^{\mathrm{in}}_\ell|\bigr)
\operatorname{tr}\!\bigl(O(3|\psi^{\mathrm{out}}_\ell\rangle\!\langle\psi^{\mathrm{out}}_\ell|-I)\bigr).
$$

The $(3|\psi\rangle\langle\psi|-I)$ factor is exactly the local classical-shadow inverse channel.

### 4. Threshold small coefficients away

Naively keeping all low-weight coefficients is too noisy. The key nonlinear step is to keep only those empirical coefficients whose magnitude exceeds a weight-dependent threshold. This produces a sparse learned operator

$$
\widehat{O'} = \sum_{|P|\le k}\hat\alpha_P(O)P
$$

with $\hat\alpha_P(O)=0$ unless the estimated coefficient is large enough to survive the filter.

### 5. Predict by evaluating local marginals

Given a new input state $\rho$, output

$$
h(\rho,O)=\operatorname{tr}(\widehat{O'}\rho).
$$

Because $\widehat{O'}$ has degree at most $k=O(\log(1/\varepsilon))$, this depends only on $k$-body reduced density matrices of $\rho$.

## Key results

### Main learnability theorem

For locally flat state distributions $\mathcal D$ and bounded-degree observables $O$ with $\|O\|\le 1$, the learned predictor satisfies an average error bound of the form

$$
\mathbb E_{\rho\sim\mathcal D}
\left|h(\rho,O)-\operatorname{tr}(O\mathcal E(\rho))\right|^2
\le
\varepsilon + \max\!\bigl(\|\overline{O'}\|^2,1\bigr)\varepsilon',
$$

where $\overline{O'}$ is the low-degree truncation of $O' = \mathcal E^\dagger(O)$.

The sample complexity is polylogarithmic in $n$ for constant precision, and more generally quasi-polynomial in $1/\varepsilon$:

$$
N = \log(n)\cdot \min\!\left\{2^{O(\log(1/\varepsilon)(\log\log(1/\varepsilon)+\log(1/\varepsilon')))},\; 2^{O(\log(1/\varepsilon)\log n)}\right\}.
$$

The runtime is polynomial in $n$ and quasi-polynomial in $1/\varepsilon$.

### Improved Hamiltonian optimization

A major technical ingredient is an improved algorithm for maximizing or minimizing a local Hamiltonian over product states. For a $k$-local Hamiltonian $H=\sum_P \alpha_P P$, they prove product-state approximation bounds depending on an $\ell_r$ norm of the Pauli coefficients with

$$
r=\frac{2k}{k+1}<2.
$$

This is the bridge to the norm inequalities below.

### Quantum Bohnenblust--Hille inequality

They derive a quantum analogue of the Bohnenblust--Hille inequality, bounding the Pauli-coefficient norm of a local observable by its operator norm. For bounded-degree observables this becomes an $\ell_1$-type bound strong enough to show that most low-degree Pauli coefficients must be small.

That sparsity statement is what makes the thresholding step work.

### Sample-optimal bounded-degree shadow learning

As a byproduct, the same norm inequality sharpens the sample complexity of [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|classical shadows]] for bounded-degree observables to the optimal logarithmic dependence on $n$.

## Comparison with prior work

| Work | What it learns | Assumptions on the process | Computational status |
|---|---|---|---|
| Full process tomography | full CPTP map | none | exponential in $n$ |
| [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Classical shadows]] | observables of a state | state copies only | efficient for good shadow norm |
| [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes|Power of Data]] | prediction advantage landscape | labeled data from quantum model | framework / diagnostics |
| This paper | local properties of $\mathcal E(\rho)$ | locally flat input ensemble; bounded-degree observables | classically efficient even for very complex $\mathcal E$ |
| [[Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) — Paper Notes|Many-body ML]] | phases / local properties in structured families | gapped many-body families | classically efficient on a narrower but more physical family |

## Limits / caveats

- The guarantee is **average-case over $\rho\sim\mathcal D$**, not worst-case over all states.
- “Locally flat” is doing real work. It forces high-degree Pauli components to wash out under the input distribution.
- Observables must be bounded-degree or otherwise structurally tame. The paper is not learning arbitrary global observables of arbitrary processes.
- Precision scaling is not pretty: the dependence on $1/\varepsilon$ is quasi-polynomial, not polynomial.
- The bound still contains a dependence on the norm of the low-degree truncation $\overline{O'}$. If Heisenberg evolution makes the observable very ugly, the practical guarantee weakens.

So the result is strong but not magic. It says that some prediction tasks are easy for deep reasons; it does not say quantum dynamics is generically learnable.

## Reusable ideas

1. [[Quantum Bohnenblust--Hille Inequality for Local Observables]] — convert local-Hamiltonian optimization guarantees into sparsity / norm control for Pauli expansions.
2. [[Thresholded Low-Degree Heisenberg Learning]] — learn only the significant low-weight Heisenberg-picture coefficients and throw the rest away.

## References within this paper

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Huang, Kueng, Preskill (2020)]] — classical-shadow primitive used to build the training data and local estimators.
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes|Huang et al. (2021)]] — earlier framing of what training data can and cannot buy in QML.
- [[Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) — Paper Notes|Huang, Kueng, Preskill (2021)]] — complementary limits on quantum-vs-classical prediction advantage.
- [[Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) — Paper Notes|Huang et al. (2022)]] — later structured-family application of the same classical-shadow / prediction worldview.
- Aaronson (2018) — shadow tomography precursor in a much less experimentally friendly form.
- Bohnenblust and Hille (1931) — classical hypercontractive inequality that this paper imports into the Pauli / local-Hamiltonian setting.

## Cross-links

### Paper notes

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]
- [[Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) — Paper Notes]]
- [[Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) — Paper Notes]]
- [[Predicting quantum channels over general product distributions (Chen-de Dios Pont-Hsieh-Huang-Lange-Li 2024) — Paper Notes]]
- [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]

### Trick cards

- [[Quantum Bohnenblust--Hille Inequality for Local Observables]]
- [[Thresholded Low-Degree Heisenberg Learning]]
- [[Classical Shadow Channel Inversion]]
- [[Projected Quantum Kernel via Local Observables]]
- [[Operator-to-State Mapping for Heisenberg Evolution]]
