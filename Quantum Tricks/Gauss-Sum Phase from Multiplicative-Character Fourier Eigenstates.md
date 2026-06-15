# Gauss-Sum Phase from Multiplicative-Character Fourier Eigenstates

## Pattern

When a finite abelian additive group also has a useful multiplicative-character theory, prepare a multiplicative-character state and apply the additive QFT. For finite fields,

$$|\chi\rangle=\frac{1}{\sqrt{q-1}}\sum_{x\in\mathbb F_q}\chi(x)|x\rangle,$$

and the additive QFT with parameter $\beta$ gives Fourier coefficients

$$\sum_x \chi(x)\zeta_p^{\operatorname{Tr}(\beta xy)}=G(\mathbb F_q,\chi,\beta y)=\chi(y^{-1})G(\mathbb F_q,\chi,\beta).$$

A diagonal correction by $\chi^2(y)$ maps the transformed state back to $|\chi\rangle$ while leaving the global eigenphase

$$G(\mathbb F_q,\chi,\beta)/\sqrt q.$$

Interfere this branch with a reference branch to estimate the Gauss-sum phase.

## How to use it

1. Specify a nontrivial multiplicative character $\chi$ compactly, usually by a generator $g$ and exponent $\alpha$.
2. Prepare $|\chi\rangle$ using discrete log in superposition plus phase kickback.
3. Apply the additive QFT $F_\beta$.
4. Apply the known diagonal phase $|y\rangle\mapsto\chi^2(y)|y\rangle$.
5. Estimate the induced eigenphase with a reference state or standard phase-estimation-style interference.

## Why it matters

This converts a global exponential-size sum into the eigenphase of an efficiently implementable unitary. The sum is not accessed by sampling its terms; it is accessed through an exact Fourier identity between additive and multiplicative characters.

## Use when

- The target quantity is a character sum with known magnitude but unknown phase.
- The QFT of the prepared state has coefficients equal to the desired sum times a known character factor.
- The character phases can be computed coherently, or at least applied as diagonal gates.

## Source paper

- [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes|van Dam-Seroussi (2002)]].

## Related

- [[On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes]] — uses this Gauss-sum phase primitive inside a Potts-partition-function algorithm via irreducible cyclic-code weights.
- [[Exact Weight Recovery by Divisibility-Gap Rounding]]
- [[Fourier State as Eigenstate of Addition]]
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]]
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]]
