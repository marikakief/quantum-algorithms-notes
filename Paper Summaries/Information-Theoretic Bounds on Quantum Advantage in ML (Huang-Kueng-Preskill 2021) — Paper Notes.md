> **Source:** Hsin-Yuan Huang, Richard Kueng, John Preskill, *Information-theoretic bounds on quantum advantage in machine learning*, arXiv:2101.02464, Physical Review Letters **126**, 190505 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2101.02464) · [PRL](https://doi.org/10.1103/PhysRevLett.126.190505)
> **Tags:** #quantum-ml #learning-theory #quantum-advantage #information-theory #shadow-tomography

---

## The computational problem

The paper studies supervised prediction of expectation values produced by an unknown quantum process. An input $x\in\{0,1\}^n$ prepares $|x\rangle\langle x|$, an unknown CPTP map $\mathcal E$ acts on it, and the target function is

$$f_{\mathcal E}(x)=\operatorname{tr}\!\left(O\,\mathcal E(|x\rangle\!\langle x|)\right),$$

where $O$ is a known $m$-qubit observable with $\|O\|\le 1$.

The question is not runtime advantage. It is query/sample advantage: how many times must a learner access $\mathcal E$ to predict $f_{\mathcal E}$?

The paper compares:

- **Quantum ML:** the learner can access $\mathcal E$ coherently and may keep quantum memory.
- **Classical ML:** the learner runs $\mathcal E$, performs measurements, and stores only classical data.
- **Restricted classical ML:** an even weaker classical learner that only measures the final observable $O$.

## What the paper does

It draws a sharp line between average-case and worst-case prediction.

For **average-case error over an input distribution**, any query advantage of quantum ML over even restricted classical ML is at most polynomial. So if someone claims an exponential QML advantage for typical prediction error, this paper says: check the model very carefully.

For **worst-case prediction over many incompatible observables**, exponential query advantage is possible. A quantum learner with quantum memory can predict all $4^n$ Pauli expectations of an unknown $n$-qubit state using $O(n)$ copies for constant error, while any classical learner needs exponentially many copies.

My assessment: this is one of Robert's clean foundational papers because it separates two stories that QML often muddles. Average-case learning from data is much less friendly to exponential quantum advantage; worst-case property prediction can still have genuine quantum-memory separations.

## The algorithm / construction

### Average-case simulation by restricted classical learning

Suppose a quantum learner uses $N_Q$ queries to $\mathcal E$ and, with high probability, outputs $h_Q$ satisfying

$$\sum_x D(x)|h_Q(x)-f_{\mathcal E}(x)|^2\le \varepsilon.$$

The proof constructs a restricted classical learner by covering the function class induced by CPTP maps with a maximal packing net. If too many well-separated functions were learnable by the quantum learner from too few queries, the learner would transmit too much information about the hidden process. Holevo information bounds prevent that.

The classical learner only samples inputs from $D$, queries $\mathcal E$, measures $O$, and uses empirical risk minimisation over the packing/net class.

### Worst-case Pauli prediction with quantum memory

For the separation, take an unknown $n$-qubit state $\rho$ and ask for estimates of many Pauli expectations

$$p_i=\operatorname{tr}(P_i\rho).$$

The quantum procedure has two pieces.

1. **Magnitude estimation by Bell measurements.** Measuring $\rho\otimes\rho$ in the Bell basis simultaneously diagonalises $P\otimes P$ for every Pauli $P$. Since

   $$\operatorname{tr}((P\otimes P)\rho^{\otimes 2})=\operatorname{tr}(P\rho)^2,$$

   the same two-copy measurement data estimates $|p_i|$ for all requested Paulis.

2. **Sign estimation using quantum memory.** For Paulis whose magnitude is large enough, keep additional copies of $\rho$ in quantum memory and measure signs sequentially. The large-magnitude condition limits how badly the residual measurements can conflict, so the procedure can recover signs without paying separately for all $M$ Paulis.

This is where the exponential separation lives: coherent access/quantum memory lets the learner avoid immediately converting every copy of $\rho$ into a classical record.

## Key results

### Theorem 1: average-case prediction has no exponential query advantage

Fix an input distribution $D$, an observable $O$ on $m$ qubits with $\|O\|\le 1$, and a family $F$ of CPTP maps from $n$ input qubits to $m$ output qubits. If a quantum ML model accesses $\mathcal E\in F$ $N_Q$ times and outputs $h_Q$ with high probability such that

$$\sum_{x\in\{0,1\}^n}D(x)|h_Q(x)-\operatorname{tr}(O\mathcal E(|x\rangle\langle x|))|^2\le \varepsilon,$$

then a restricted classical ML model accesses $\mathcal E$ only

$$N_C=O(mN_Q/\varepsilon)$$

times and outputs $h_C$ with high probability such that

$$\sum_{x\in\{0,1\}^n}D(x)|h_C(x)-\operatorname{tr}(O\mathcal E(|x\rangle\langle x|))|^2=O(\varepsilon).$$

So average-case query advantage is polynomially bounded.

### Theorem 2 / Theorem 4: quantum learner for Pauli prediction

For any $M$ Pauli operators $P_1,\ldots,P_M$, a quantum procedure estimates all

$$\operatorname{tr}(P_i\rho)$$

to additive error $\varepsilon$ with probability at least $1-\delta$ using

$$N_Q=O\!\left(\frac{\log(M/\delta)}{\varepsilon^4}\right)$$

copies of $\rho$.

Taking $M=4^n$ and constant $\varepsilon,\delta$ gives $N_Q=O(n)$ copies for all Pauli observables.

### Theorem 3: classical lower bound

Any classical ML model must use

$$N_C\ge 2^{\Omega(n)}$$

copies of $\rho$ to predict expectation values of all Pauli observables up to small error with constant success probability. The lower bound still allows adaptive POVM choices depending on previous measurement outcomes.

## Comparison with prior work

| Question | Earlier picture | This paper's contribution |
|---|---|---|
| Can QML beat classical ML on prediction? | Many claims conflate hard simulation with hard learning | Average-case query advantage is at most polynomial |
| Can quantum memory help learning? | Known in tomography / communication settings | Exponential separation for worst-case Pauli prediction |
| Relation to [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|classical shadows]] | Classical shadows efficiently reuse single-copy measurements | Single-copy classical data is still exponentially weaker than quantum memory for all Paulis |
| Source of separation | Often attributed vaguely to Hilbert-space dimension | Specifically: coherent memory + incompatible observables + worst-case guarantee |

## Limits / caveats

- The main upper/lower bounds are about **query complexity**, not total runtime. A polynomial query simulation does not mean the resulting classical algorithm is computationally efficient in every representation.
- The positive average-case result is for squared error over a distribution $D$. It does not rule out worst-case, distribution-shift, or cryptographic-style separations.
- The worst-case separation is a property-prediction task over all Pauli observables. It is mathematically clean, but not automatically a practical ML advantage.
- The quantum upper bound uses quantum memory / coherent prediction. This is not a near-term protocol.
- The paper is best read as a constraint on advantage claims, not as evidence that useful QML is impossible.

## Reusable ideas

1. [[Average-Case vs Worst-Case QML Advantage Test]] — separate typical prediction error from worst-case property prediction before taking an exponential advantage claim seriously.
2. [[Bell Difference Sampling for Magnitude Estimation]] — use two-copy Bell measurements to estimate squared Pauli expectations for many Paulis at once.
3. [[Holevo Information Bottleneck for Learning Lower Bounds]] — convert learning from a quantum process into an information-transmission problem and bound the learner's accessible information.

## References within this paper

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Huang, Kueng, Preskill (2020)]] — classical shadows; the baseline for single-copy randomized measurement prediction.
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2018)]] — earlier shadow tomography and learnability perspective.
- Aaronson (2007/2018 quantum PAC learning line) — learning quantum states and measurements under distributional assumptions.
- Holevo's theorem — information-theoretic cap used in the average-case lower-bound machinery.
- Gentle measurement / quantum union-bound ideas — implicit in the sign-recovery step with quantum memory.

## Cross-links

### Paper notes

- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]] — predecessor; single-copy classical-shadow protocol.
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — later QML advantage framework; complementary message about data often helping classical learners.
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — earlier PAC-learning viewpoint.
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — shadow tomography predecessor.
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — later two-copy shadow tomography; reuses Bell-sampling magnitude ideas.

### Trick cards

- [[Average-Case vs Worst-Case QML Advantage Test]]
- [[Bell Difference Sampling for Magnitude Estimation]]
- [[Holevo Information Bottleneck for Learning Lower Bounds]]
