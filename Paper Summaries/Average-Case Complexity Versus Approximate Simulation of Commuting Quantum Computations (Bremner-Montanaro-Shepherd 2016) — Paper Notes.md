> **Source:** Michael J. Bremner, Ashley Montanaro, Dan J. Shepherd, *Average-case complexity versus approximate simulation of commuting quantum computations*, Phys. Rev. Lett. **117**(8):080501, 2016; arXiv:1504.07999
> **Links:** [arXiv](https://arxiv.org/abs/1504.07999) · [PRL](https://doi.org/10.1103/PhysRevLett.117.080501)
> **Tags:** #quantum-supremacy #IQP #complexity-theory #polynomial-hierarchy #sampling #average-case-hardness #Ising-model

---

## The problem

Can IQP circuits be approximately simulated classically, up to *additive error* in total variation distance?

The earlier [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] result showed hardness only for *multiplicative error* — a physically unrealistic noise model. Real devices produce errors that shift probabilities additively, not multiplicatively. This paper closes that gap: it shows that even approximate (additive-error) classical sampling from IQP output distributions is hard, conditional on average-case complexity conjectures.

## What the paper does

The main result (Theorem 1) is:

**If either Conjecture 2 or Conjecture 3 holds, then any classical polynomial-time algorithm that samples from the output distribution of an arbitrary IQP circuit to within $\ell_1$ error $1/192$ implies a BPPNP algorithm for any problem in P#P — collapsing the polynomial hierarchy to its third level.**

This is the IQP analogue of Aaronson-Arkhipov's approximate hardness result for [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes|BosonSampling]], but with a mathematically simpler structure that lets them *prove* the anticoncentration property (which remains conjectural for BosonSampling).

---

## The argument

The proof has three ingredients: (1) Stockmeyer's counting theorem, (2) random self-reducibility of IQP, and (3) anticoncentration bounds.

### Ingredient 1: From approximate sampler to multiplicative estimator (Lemma 4)

Given a classical sampler $A$ that approximates any IQP circuit's output distribution to $\ell_1$ error $\epsilon$:

- For an IQP circuit $C$, define $C_x$ by appending $X$ gates for each bit $x_i = 1$. This randomly permutes the output probabilities: $\Pr[C_0 \text{ outputs } y] = |\langle 0 | C_y | 0 \rangle|^2$.
- Run the sampler $A$ on $C_0$, then use [[Stockmeyer's Counting Theorem]] to estimate individual output probabilities $q_{0y}$ to multiplicative error $1/\text{poly}(n)$.
- By Markov's inequality, the $\ell_1$ guarantee means $|p_{0y} - q_{0y}| \leq \epsilon / (2^n \delta)$ for at least a $1 - \delta$ fraction of outcomes $y$.
- Combining: if $|\langle 0|C_x|0\rangle|^2 = \Omega(2^{-n})$, we get a multiplicative approximation.

This is the same structure as the BosonSampling argument, but the X-gate trick replaces the unitary-averaging argument for the permanent. The IQP version is cleaner because appending $X$ gates to an IQP circuit produces another IQP circuit.

### Ingredient 2: Average-case hardness conjectures

The argument converts the approximate sampler to a FBPPNP algorithm that multiplicatively approximates certain quantities. Two candidate hard problems:

**Conjecture 2 (Ising partition function):** For the partition function $Z(\omega) = \sum_{z \in \{\pm 1\}^n} \omega^{\sum_{i<j} w_{ij} z_i z_j + \sum_k v_k z_k}$ with $\omega = e^{i\pi/8}$ and weights $w_{ij}, v_k$ uniform in $\{0, \ldots, 7\}$, it is #P-hard to approximate $|Z_R|^2$ to multiplicative error $1/4 + o(1)$ for a $1/24$ fraction of instances.

**Conjecture 3 (Degree-3 polynomial gap):** For a uniformly random degree-3 polynomial $f : \{0,1\}^n \to \{0,1\}$ over $\mathbb{F}_2$, it is #P-hard to approximate $\text{ngap}(f)^2$ to multiplicative error $1/4 + o(1)$ for a $1/24$ fraction of polynomials, where $\text{ngap}(f) = \text{gap}(f)/2^n$.

Both quantities are already #P-hard *in the worst case* (proven in Goldberg-Guo 2014 and Appendix D respectively). The conjectures ask that worst-case hardness lifts to average-case. This is the IQP analogue of the permanent-of-Gaussians conjecture for BosonSampling.

The connection to IQP is via [[IQP Amplitudes as Ising Partition Functions]]: $\langle 0^n | C_I | 0^n \rangle = Z_R / 2^n$ for appropriately constructed IQP circuits, and $\langle 0^n | C_f | 0^n \rangle = \text{ngap}(f)$ for circuits built from $Z$, $CZ$, $CCZ$ gates.

### Ingredient 3: Anticoncentration (the key technical advance)

The argument needs IQP amplitudes to not be too concentrated near zero — otherwise the additive-to-multiplicative conversion fails. Formally, need $\Pr[|\langle 0|C|0\rangle|^2 \geq \alpha \cdot 2^{-n}] \geq p$ for constants $\alpha, p > 0$.

They prove this using the Paley-Zygmund inequality. For a non-negative random variable $R$:

$$\Pr[R \geq \alpha\, \mathbb{E}[R]] \geq (1-\alpha)^2 \frac{\mathbb{E}[R]^2}{\mathbb{E}[R^2]}$$

The key technical result is **Lemma 11**: for both the Ising partition function family and random degree-3 polynomials,

$$\mathbb{E}[|\langle 0|C|0\rangle|^4] \leq 3 \cdot 2^{-2n}$$

Since $\mathbb{E}[|\langle 0|C|0\rangle|^2] = 2^{-n}$ (by the random X-gate averaging), Paley-Zygmund gives:

$$\Pr[|\langle 0|C|0\rangle|^2 \geq 2^{-n-1}] \geq 1/12$$

This is the result that Aaronson-Arkhipov could not prove for BosonSampling — their "permanent anticoncentration conjecture" remains open. The IQP structure is algebraically simpler: the fourth moment calculation reduces to counting quadruples $(w,x,y,z)$ of bit strings satisfying certain modular constraints, and most such quadruples contribute zero due to the random phases.

### Putting it together

Combining via Corollary 10: if a classical sampler achieves $\ell_1$ error $\epsilon = 1/192$, then there's a FBPPNP algorithm that multiplicatively approximates $|Z_R|^2$ (resp. $\text{ngap}(f)^2$) on a $1/24$ fraction of instances. Assuming either conjecture, this means computing #P-hard quantities in BPPNP, which collapses PH.

---

## Key results

| Statement | Consequence |
|---|---|
| Additive $\ell_1$ error $< 1/192$ classical simulation of IQP | PH collapses (conditional on Conjectures 2 or 3) |
| $\mathbb{E}[\text{ngap}(f)^4] \leq 3 \cdot 2^{-2n}$ | Anticoncentration for degree-3 polynomial IQP |
| $\mathbb{E}[|Z_R|^4] \leq 3 \cdot 2^{2n}$ | Anticoncentration for random Ising IQP |
| Worst-case #P-hardness of $\text{ngap}(f)^2$ to mult. error $< 1/2$ | Supports plausibility of average-case conjecture |

---

## Comparison with prior work

| Work | Error model | Conclusion | Anticoncentration |
|---|---|---|---|
| [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes\|Bremner-Jozsa-Shepherd (2011)]] | Multiplicative ($c < \sqrt{2}$) | PH collapses | Not needed |
| [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes\|Aaronson-Arkhipov (2011)]] | Additive ($\ell_1$) | PH collapses (conditional on PAC + PGC) | Conjectured |
| **This paper (2016)** | **Additive ($\ell_1 < 1/192$)** | **PH collapses (conditional on Conj. 2 or 3)** | **Proven** |

The main advance over Bremner-Jozsa-Shepherd 2011: the error model is now physically relevant. The main advance over Aaronson-Arkhipov 2011: anticoncentration is proven rather than conjectured, and the remaining conjecture (average-case hardness) is arguably simpler.

---

## Gate sets and implementation

Two concrete circuit families:

1. **$C_I$ (Ising circuits):** Built from $\sqrt{CZ}$ (= $\text{diag}(1,1,1,i)$) and $T$ gates. Requires all-to-all connectivity on the complete graph.
2. **$C_f$ (polynomial circuits):** Built from $Z$, $CZ$, and $CCZ$ gates. Also requires non-local interactions for random degree-3 polynomials.

Both families need non-local gates, which is experimentally challenging. The authors note progress in digital simulation with ion traps and superconducting circuits, and suggest cavity buses as a path to non-local $ZZ$ interactions.

---

## Limits / caveats

- **The conjectures are unproven.** Both Conjectures 2 and 3 ask for average-case hardness of quantities known to be worst-case hard. No current technique lifts worst-case to average-case for these problems. The paper provides evidence (worst-case hardness, random self-reducibility over *some* distribution) but the gap remains.
- **The $1/192$ error tolerance is small.** Real devices would need total variation distance well below this threshold as the system size grows. Whether this is physically achievable depends on the error rates and how they scale.
- **Non-local gates required.** The random IQP families used here need complete-graph connectivity. Restricting to geometrically local interactions would need new anticoncentration results. Later work on MBQC-based IQP (Shepherd 2009, Hoban et al. 2014) may help.
- **Still conditional on PH not collapsing.** Same caveat as all quantum supremacy arguments.

---

## Reusable ideas

1. [[Paley-Zygmund Anticoncentration for Quantum Amplitudes]] — use the Paley-Zygmund inequality with a fourth-moment bound to prove that IQP amplitudes are not too concentrated near zero. Requires computing $\mathbb{E}[|\langle 0|C|0\rangle|^4]$ and showing it's at most $O(\mathbb{E}[|\langle 0|C|0\rangle|^2]^2)$. The key move: the fourth moment sum decomposes into quadruples of bit strings, most of which contribute zero because random phases cancel.

2. [[X-Gate Random Self-Reduction for IQP]] — appending random $X$ gates to an IQP circuit permutes the output probabilities without leaving the IQP class. This gives a clean self-reduction: the problem of approximating $|\langle 0|C|0\rangle|^2$ for a random circuit and random $x$ reduces to sampling from the output distribution of a random IQP circuit. Unlike the BosonSampling setting (where random unitaries take you outside the linear-optical class), random bit flips are "free" for IQP.

---

## References within this paper

- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] — direct predecessor; proved multiplicative-error hardness and post-IQP = PP
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes|Aaronson-Arkhipov (2011)]] — BosonSampling; developed the additive-error framework this paper generalises
- Shepherd & Bremner (2009) — introduced IQP circuits
- Goldberg & Guo (2014) — worst-case #P-hardness of Ising partition functions at complex temperatures
- Stockmeyer (1985) — the counting theorem converting approximate sampling to multiplicative estimation
- Fefferman & Umans (2015) — independent related work using Quantum Fourier Sampling
- Fujii & Morimae (2013) — commuting circuits and Ising partition function complexity

---

## Cross-links

### Paper notes
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — direct predecessor; this paper strengthens the 2011 result from multiplicative to additive error
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]] — the BosonSampling parallel; same proof structure but IQP proves its anticoncentration
- [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes]] — classical FPTAS for bounded-degree IQP amplitudes; the complementary "easy" regime
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]] — stabilizer circuits are classically easy; contrast with IQP hardness

### Trick cards
- [[Paley-Zygmund Anticoncentration for Quantum Amplitudes]]
- [[X-Gate Random Self-Reduction for IQP]]
- [[Hadamard Gadget for IQP Reduction]] — used in the worst-case hardness proof (Appendix D)
- [[Post-Selection Boosting for Hardness of Sampling]] — the template from the 2011 paper, which this paper extends
- [[IQP Amplitudes as Ising Partition Functions]] — the connection between IQP and the Ising model used throughout
