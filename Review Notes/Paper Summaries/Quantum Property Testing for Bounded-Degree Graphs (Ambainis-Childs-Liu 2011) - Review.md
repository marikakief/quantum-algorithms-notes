# Review: Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011)

Source note: [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]]

## Primary Sources Checked

- Ambainis, Childs, Liu, "Quantum property testing for bounded-degree graphs", arXiv:1012.3174, RANDOM 2011: https://arxiv.org/abs/1012.3174
- Ambainis element-distinctness quantum walk checked as the collision subroutine background.

## Verdict

Minor issues. The note accurately conveys the main template: derandomize classical bounded-degree graph testers, express walk endpoints as deterministic functions, then apply element distinctness. The lower-bound discussion is also directionally right. The main needed improvements are parameter hygiene and avoiding exact-looking `N^{1/3}` claims where the expansion tester has auxiliary parameters.

## Line-Anchored Comments

- Lines 25-28: main result is correct.

  The source abstract states quantum algorithms in time `O(N^{1/3})` and an `Omega(N^{1/4})` quantum lower bound for expansion, with suppressed dependencies on `d`, `epsilon`, `alpha`, and logarithmic factors.

- Lines 43-47: derandomization explanation is good.

  This is the key reason the element-distinctness subroutine can be coherently queried. Keep it prominent.

- Lines 56-60: expansion cost needs more parameter care.

  The note introduces `mu`, `K=N^{1/2+mu}`, and a collision threshold `M`, then writes a cost exponent. That exponent should be checked directly from the theorem; as written it is too easy to confuse `N^{1/3+3mu}` with the final headline `N^{1/3}`.

- Lines 66-75: table overcompresses the expansion result.

  The expansion upper bound should be stated as `N^{1/3+O(mu)}` or with the paper's parameter tradeoff before saying it can be made arbitrarily close to `N^{1/3}`. The current table suggests a clean fixed `tilde O(N^{1/3})` theorem for all parameter settings.

- Lines 81-90: lower-bound sketch is useful but dense.

  Add a sentence saying why the ordinary collision lower bound cannot just be copied: graph-oracle queries reveal structured neighbor information, so the acceptance polynomial has to be averaged over graph distributions rather than independent list entries.

- Line 108: "bipartiteness lower bound is open"

  This is accurate as a paper-level caveat. If the vault aims to track current status, add "as of this paper" unless a later source is checked.

## Missing or Suggested Additions

- Add model labels: bounded-degree adjacency-list property testing, not dense adjacency-matrix graph testing.
- State that the quantum speedup is polynomial but not known to be tight for either bipartiteness or expansion in this paper.
