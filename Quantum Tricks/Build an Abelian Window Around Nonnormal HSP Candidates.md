# Build an Abelian Window Around Nonnormal HSP Candidates

## Pattern

When the nonnormal hidden subgroups in a nonabelian group all lie inside a small abelian subgroup, construct that abelian subgroup from the available generators and solve the restricted HSP there.

For $P_{p,r}=\langle x,y\mid x^{p^r}=y^p=e,\ yx=x^{p^{r-1}+1}y\rangle$, the nonnormal candidates are

$$
\langle x^{t p^{r-1}}y\rangle,
\qquad t\in\mathbb Z_p.
$$

They lie in

$$
\langle x^{p^{r-1}},y\rangle\cong\mathbb Z_p\times\mathbb Z_p
$$

for odd $p$. In a black-box presentation, Inui--Le Gall construct suitable generators $X^{p^{r-1}}$ and $Y'$ for this subgroup, then run abelian Fourier sampling.

## Why it helps

This avoids implementing a full nonabelian Fourier transform or PGM. The nonabelian structure is used only to build an abelian “window” containing the candidates.

## Where it appears

- [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes]] — handles the nonnormal subgroups of $P_{p,r}$.

## Reuse checklist

- Identify all nonnormal candidate subgroups.
- Find an abelian subgroup containing them.
- Show the needed abelian generators can be built from the given black-box generators.
- Restrict the original hiding function to the abelian subgroup and solve by abelian HSP.
