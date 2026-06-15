# Review: Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022)

Source note: [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2110.04993
- Quantum journal page linked in the source note.

## Verdict

Minor issues. This is a good summary of the maximal-solution and local-convexity story. The main problems are a misleading cross-link/citation at line 31 and a few places where the note should emphasize how restrictive the proven small-norm regime is.

## Line-Anchored Comments

- Lines 9-19: The phase vector is written as a full `d+1` tuple, but the optimization variables are the symmetric half, `e_d = ceil((d+1)/2)`. Spell this out early because the variable count is central to why the Hessian is nonsingular.

- Lines 27-31: The summary of the contribution is accurate. However, line 31 points to `QET-U` while naming "Dong, Meng, Whaley & Lin (2021)." That is the phase-factor-evaluation/QSPPACK paper, not QET-U. This should be corrected to avoid a wrong genealogy.

- Lines 37-42: The uniqueness statement is for symmetric phase factors in the chosen domain and for an admissible polynomial/completion pair. Keep "for each admissible pair" attached to uniqueness, since the next section correctly explains that many complementary pairs/global minima exist.

- Lines 53-67: The maximal-solution discussion is strong. It is worth adding that "inside the unit disc" is a root-grouping rule for the Laurent polynomial completion, not a physical spectral condition.

- Lines 69-83: This is the key caveat: the provable local strong convexity/convergence result assumes `||f||_infty = O(1/d)`. The note says this later, but it should be highlighted in the theorem summary too.

- Lines 106-111: The comparison table includes "Capitalisation (Chao et al. 2020)"; check spelling and method name against that paper. Also, saying the small-norm condition "degrades query complexity" is too quick: scaling the polynomial can affect downstream success/amplification, but phase computation itself is classical preprocessing.

## Missing or Suggested Additions

- Add a one-line explanation of why non-symmetric phases create flat directions: more variables than interpolation constraints, hence a singular Hessian at global minima.

- Add a pointer to the empirical gap: practical QSPPACK works for targets with norm close to one, while the theorem proves convergence only in a much smaller ball.

