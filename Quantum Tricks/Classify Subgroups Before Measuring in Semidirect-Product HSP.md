# Classify Subgroups Before Measuring in Semidirect-Product HSP

## Pattern

For a specific nonabelian group family, first classify all subgroups up to a small set of structural forms. Then design the quantum part only for the remaining cases that are not already handled by normal or abelian HSP algorithms.

In semidirect products this often means:

1. list cyclic and two-generator subgroups;
2. identify which subgroups are normal;
3. isolate a small family of nonnormal candidates;
4. solve the normal cases by a normal-HSP routine and the nonnormal cases with a custom abelian or Fourier test.

## Why it helps

A general nonabelian HSP measurement can be hard to implement. A subgroup classification can shrink the unknown space enough that standard tools become adequate.

## Where it appears

- [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes]] — classifies subgroups of $P_{p,r}$; only $\langle x^{t p^{r-1}}y\rangle$ are nonnormal.
- [[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes]] — uses subgroup forms in restricted semidirect products.

## Reuse checklist

- Is the group family narrow enough for a complete subgroup classification?
- Are most subgroups normal or abelian?
- Can the remaining nonnormal cases be embedded in a tractable abelian subgroup?
