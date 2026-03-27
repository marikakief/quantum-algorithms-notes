
> **Source:** Aaronson & Arkhipov, arXiv:1011.3245; classical result in quantum optics
> **Tags:** #trick #linear-optics #BosonSampling #permanent #bosons #second-quantization

## What it does

Computes the transition amplitude for $n$ indistinguishable bosons through a linear-optical unitary directly as the permanent of a submatrix.

## The trick

Let $U$ be an $m \times m$ unitary acting on modes, and suppose $n$ photons enter in modes $r_1, \ldots, r_n$ (with possible repeats if multiple photons enter the same mode). The amplitude for them to exit in modes $t_1, \ldots, t_n$ (again with repeats allowed) is:

$$\langle t_1, \ldots, t_n | U^{\otimes n} | r_1, \ldots, r_n \rangle = \frac{\text{Per}(U_{\{t_i\}, \{r_j\}})}{\sqrt{\prod_k s_k! \cdot \prod_k r_k!}}$$

where $U_{\{t_i\}, \{r_j\}}$ is the $n \times n$ matrix with entry $(i,j)$ equal to $U_{t_i, r_j}$, and $s_k$, $r_k$ are the occupation numbers at each mode in output and input respectively.

For the standard BosonSampling setup (one photon per input mode, measure output occupation numbers $S$ with $s_k \in \{0,1\}$ for the collision-free case):

$$\Pr[S = T] = |\text{Per}(U_{T, [n]})|^2$$

where $U_{T,[n]}$ selects rows $T = (t_1, \ldots, t_n)$ and columns $1, \ldots, n$.

The permanent arises because bosons are symmetric under permutation: the amplitude sums over all $n!$ permutations of the photon paths, each contributing $\prod_i U_{t_{\sigma(i)}, r_i}$, giving exactly $\text{Per}(U_{T,[n]}) = \sum_{\sigma \in S_n} \prod_i U_{t_{\sigma(i)}, i}$.

(Compare: for fermions the analogous formula involves the determinant — the antisymmetric sum — which can be computed in polynomial time. The permanent is \#P-hard.)

## When to reach for it

- Any analysis of linear-optical quantum circuits: the output statistics are controlled by permanents.
- Proving hardness of classically simulating boson computers: the connection to \#P-hard permanents is the starting point.
- Understanding why bosons are computationally harder to simulate than fermions (determinant vs permanent).
- Designing or analyzing quantum optics experiments for computational advantage.

## Complexity

Evaluating the permanent of an $n \times n$ matrix exactly is \#P-hard (Valiant 1979) and the best known classical algorithm runs in $O(2^n n)$ time (Ryser's formula). Approximately computing it to multiplicative error is also conjectured \#P-hard (the Permanent-of-Gaussians Conjecture). This is the computational core of BosonSampling hardness.

Sampling from the distribution (implementing a boson computer physically) takes $O(m^2)$ beamsplitters by the Reck decomposition — physically efficient.

## Caveat

- The formula assumes ideal, indistinguishable photons. With partially distinguishable photons, the amplitude is a mixture of permanents and determinants (interpolating between bosonic and distinguishable-particle statistics), and the hardness argument weakens.
- Losses and noise in physical implementations change the effective model. Connecting noisy BosonSampling to the permanent-based hardness requires additional arguments (and remains an active research area).
- The formula holds for any linear-optical unitary (beamsplitters + phaseshifters). Nonlinear optical interactions go beyond this model.

## Related notes
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]]
- [[Gaussian Permanent Hiding via Haar Random Embedding]]
- [[Submatrix-of-Haar-Unitary to Gaussian Approximation]]
- [[Post-Selection Boosting for Hardness of Sampling]]
