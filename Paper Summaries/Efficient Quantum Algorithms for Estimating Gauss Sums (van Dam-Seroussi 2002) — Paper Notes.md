# Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes

> **Source:** Wim van Dam and Gadiel Seroussi, *Efficient quantum algorithms for estimating Gauss sums*, arXiv:quant-ph/0207131
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0207131) · [PDF](../sources/zoo-algebraic-number-theoretic/090_quant-ph_0207131.pdf)
> **Tags:** #quantum-algorithms #gauss-sums #finite-fields #dirichlet-characters #qft #number-theory

---

## Metadata

- **Authors:** Wim van Dam; Gadiel Seroussi
- **Year:** 2002
- **arXiv:** [quant-ph/0207131](https://arxiv.org/abs/quant-ph/0207131)
- **Zoo entry:** Quantum Algorithm Zoo #90
- **Topic:** Quantum estimation of Gauss-sum phases over finite fields and rings.
- **Status in vault:** Added from Algorithm Zoo algebraic/number-theoretic batch; full PDF read from local source.

## Computational problem

Given a finite field $\mathbb F_{p^r}$, a nontrivial multiplicative character $\chi$, and a nonzero additive-character parameter $\beta$, estimate the phase $\gamma$ in

$$G(\mathbb F_{p^r},\chi,\beta)=\sum_{x\in\mathbb F_{p^r}}\chi(x)\zeta_p^{\operatorname{Tr}(\beta x)}=\sqrt{p^r}\,e^{i\gamma}.$$

The input is explicit, not a black-box function: $\chi$ is specified by a primitive field element $g$ and an exponent $\alpha$ with $\chi(g^j)=\zeta_{p^r-1}^{\alpha j}$. The paper also treats ring sums over $\mathbb Z/n\mathbb Z$ with Dirichlet characters,

$$G(\mathbb Z/n\mathbb Z,\chi,\beta)=\sum_{x\in\mathbb Z/n\mathbb Z}\chi(x)\zeta_n^{\beta x}.$$

## What the paper does

van Dam and Seroussi give efficient quantum algorithms for estimating the phase of Gauss sums over finite fields and over rings $\mathbb Z/n\mathbb Z$. The method is not the standard hidden-subgroup template. It uses the fact that the additive QFT maps a multiplicative-character state to the inverse character state with a Gauss-sum phase attached.

For finite fields, the algorithm runs in

$$O\!\left(\frac{1}{\varepsilon}\operatorname{polylog}(p^r)\right)$$

time and estimates $\gamma$ with expected error below $\varepsilon$. For rings, it first reduces the character into prime-power factors, handles trivial and imprimitive parts classically, and applies the same phase-estimation idea only to primitive Dirichlet-character components.

## Algorithmic structure

### 1. Prepare the multiplicative-character state

For a character $\chi$ over $\mathbb F_{p^r}$,

$$|\chi\rangle=\frac{1}{\sqrt{p^r-1}}\sum_{x\in\mathbb F_{p^r}}\chi(x)|x\rangle.$$

The preparation uses:

- amplitude amplification to restrict to $\mathbb F_{p^r}^*$;
- Shor's discrete-log algorithm to compute $\log_g(x)$ in superposition;
- Fourier-basis phase kickback to write $\zeta_{p^r-1}^{\alpha\log_g(x)}$ as a phase.

This takes $\operatorname{polylog}(p^r)$ quantum time, assuming the usual efficient arithmetic over the finite field.

### 2. Use the additive QFT as a Gauss-sum phase operator

Apply the finite-field QFT

$$F_\beta:|x\rangle\mapsto \frac{1}{\sqrt{p^r}}\sum_{y\in\mathbb F_{p^r}}\zeta_p^{\operatorname{Tr}(\beta xy)}|y\rangle.$$

For $y\neq0$, the inner coefficient is

$$G(\mathbb F_{p^r},\chi,\beta y)=\chi(y^{-1})G(\mathbb F_{p^r},\chi,\beta).$$

So $F_\beta|\chi\rangle$ is proportional to the inverse-character state, with the Gauss-sum phase as the proportionality factor. A diagonal correction $|y\rangle\mapsto\chi^2(y)|y\rangle$ maps it back to $|\chi\rangle$:

$$|\chi\rangle\mapsto \frac{G(\mathbb F_{p^r},\chi,\beta)}{\sqrt{p^r}}|\chi\rangle=e^{i\gamma}|\chi\rangle.$$

### 3. Estimate the phase by interference

Run the phase operation in superposition with a reference branch:

$$\frac{|\emptyset\rangle+|\chi\rangle}{\sqrt2}\mapsto \frac{|\emptyset\rangle+e^{i\gamma}|\chi\rangle}{\sqrt2}.$$

Measurements in bases $\frac{1}{\sqrt2}(|\emptyset\rangle\pm e^{i\phi}|\chi\rangle)$ for chosen $\phi$ estimate $\gamma$. The paper states an $O(1/\varepsilon)$ sample cost for expected phase error $\varepsilon$.

### 4. Extend from fields to rings

For $\mathbb Z/n\mathbb Z$, the paper uses the Chinese remainder decomposition

$$(\mathbb Z/n\mathbb Z)^*\cong\prod_i (\mathbb Z/p_i^{r_i}\mathbb Z)^*$$

and decomposes a Dirichlet character $\chi$ into prime-power factors. The ring algorithm:

1. splits the Gauss sum into prime-power Gauss sums using CRT;
2. evaluates trivial-character cases classically;
3. reduces imprimitive characters to primitive characters of smaller conductor;
4. for each remaining primitive character, uses the QFT identity
   $$G(\mathbb Z/n\mathbb Z,\chi,\beta)=\chi^{-1}(\beta)G(\mathbb Z/n\mathbb Z,\chi,1),\qquad |G(\mathbb Z/n\mathbb Z,\chi,1)|=\sqrt n,$$
   and applies the same phase-eigenvalue method.

## Main results

- **Finite fields:** Gauss-sum phase estimation over $\mathbb F_{p^r}$ in $O((1/\varepsilon)\operatorname{polylog}(p^r))$ time.
- **Jacobi sums:** since nontrivial Jacobi sums satisfy
  $$J(\chi,\psi)=\frac{G(\chi,1)G(\psi,1)}{G(\chi\psi,1)},$$
  their phases can be estimated with the same asymptotic cost.
- **Finite rings:** Gauss-sum norm and phase over $\mathbb Z/n\mathbb Z$ can be computed/estimated in $O((1/\varepsilon)\operatorname{polylog}n)$ time after CRT and conductor reductions.
- **Classical hardness evidence:** if a classical oracle estimates $G(\mathbb F_{p^r},\chi,\beta)$ phases for arbitrary $\beta$, then discrete logarithms over $\mathbb F_{p^r}$ can be recovered classically by querying phases for $\beta=x^k$ with $k=1,2,4,\ldots$.

## Why the algorithm is interesting

The paper is an early example of a natural algebraic number-theory problem with an efficient quantum algorithm that is not simply another black-box period-finding task. The core operation is closer to “turn a Fourier transform identity into an eigenphase” than to hiding a subgroup.

The construction also explains why random sampling of summands is not enough: the terms $\chi(x)e_\beta(x)$ behave like a pseudorandom walk in the complex plane whose endpoint has norm only $\sqrt{|R|}$ after $|R|$ unit steps. Polynomially many samples in $\log|R|$ do not reveal the final direction.

## Relationship to other vault notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] supplies the discrete-log subroutine used for character-state preparation.
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]] later places Gauss-sum estimation among algebraic quantum algorithms outside the basic abelian-HSP template.
- [[Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016) — Paper Notes]] uses related finite-field character structure, but for hidden shifts of difference-set characteristic functions.
- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]] contains a different appearance of Gauss sums inside affine-group Fourier sampling, where the sums do not by themselves identify the hidden conjugate.
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]] is the source for the phase-kickback/QFT viewpoint used in the paper.

## Reusable ideas

- [[Gauss-Sum Phase from Multiplicative-Character Fourier Eigenstates]] — use an additive QFT on a multiplicative-character state, then correct by a diagonal character phase to expose a Gauss sum as a measurable eigenphase.
- [[Dirichlet-Character Reduction for Ring Gauss Sums]] — reduce ring Gauss sums by CRT, conductor, and trivial-character cases before invoking the quantum primitive only on primitive components.

## Caveats

- The paper gives strong evidence against easy classical estimation via a discrete-log reduction, but it does not prove classical hardness independent of discrete logarithm.
- Quadratic-character Gauss sums are already explicitly known; the quantum algorithm matters for general multiplicative characters and the ring cases left after reductions.
- For primitive characters over $\mathbb Z/p^r\mathbb Z$ with $r\ge2$, some Gauss sums are classically known; the paper notes that the quantum phase-estimation step is not always necessary there.

## References

- Wim van Dam and Gadiel Seroussi, *Efficient quantum algorithms for estimating Gauss sums*, arXiv:quant-ph/0207131.
- Bruce C. Berndt, Ronald J. Evans, and Kenneth S. Williams, *Gauss and Jacobi Sums*, Wiley, 1998.
