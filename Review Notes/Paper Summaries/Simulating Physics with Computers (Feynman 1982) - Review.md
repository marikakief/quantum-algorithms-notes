# Review: Simulating Physics with Computers (Feynman 1982)

Source note: [[Simulating Physics with Computers (Feynman 1982) — Paper Notes]]

## Primary Sources Checked

- Feynman, "Simulating physics with computers", International Journal of Theoretical Physics 21, 467-488 (1982), DOI: https://doi.org/10.1007/BF02650179
- Full-text mirror checked for content: https://docslib.org/doc/169310/simulating-physics-with-computers
- Shor 1996/1997 historical discussion of Feynman checked: https://arxiv.org/pdf/quant-ph/9508027

## Verdict

Major issues. The note is valuable, but it overstates Feynman's argument as a clean classical intractability explanation and contains one clear anachronistic reference error.

## Line-Anchored Comments

- Lines 9-11: "His answer is no ... the reason is that quantum mechanics requires negative probabilities"

  This is too strong. Feynman gives an informal physical argument using local simulation, negative quasi-probabilities, and Bell-type correlations. He does not prove an efficient classical simulation lower bound, and "negative probabilities" are not a complete explanation of quantum computational hardness. Better: Feynman argues that local classical probabilistic simulation cannot generally reproduce quantum correlations without allowing nonclassical, sometimes negative, intermediate quantities.

- Lines 51-59: Bell inequality discussion

  The high-level point is good. The wording should make clear that this is a locality/no-hidden-variable obstruction, not a modern complexity-theoretic lower bound against all classical algorithms. A nonlocal classical simulation with exponential resources is not excluded by Bell's argument.

- Lines 63-68: "He conjectures ... universal quantum simulator"

  Good, but distinguish "universal quantum simulator" from the later formal universal quantum computer/QTM and gate-model universality results. Feynman's paper is exploratory and does not define an algorithmic model precise enough for the later complexity claims.

- Lines 89-90: "negative Wigner function ... formal version requires Bell inequality violations or contextuality"

  This is directionally right but too compressed. Wigner negativity, contextuality, Bell nonlocality, and classical simulation hardness are related but not equivalent. If retained, say these are later formal lenses, not a single formal version of Feynman's argument.

- Line 114: "Lloyd's 1995 PRL and Deutsch-Barenco-Ekert 1995 - universality ... references [20, 21]"

  This is an anachronism. A 1982 paper cannot reference 1995 results. Remove from "References within this paper". If the goal is downstream influence, move it to a separate "Later developments" section.

## Missing or Suggested Additions

- Add one sentence that Feynman's target is exact/local physical simulation, not the modern task of approximating local observables of local Hamiltonian dynamics to inverse-polynomial error.
- Add that Lloyd (1996) later gives the formal product-formula simulation theorem for local Hamiltonians; do not present that as already present in Feynman.
