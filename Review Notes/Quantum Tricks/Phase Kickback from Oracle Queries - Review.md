# Review: Phase Kickback from Oracle Queries

Source note: [[Phase Kickback from Oracle Queries]]

## Primary Sources Checked

- Deutsch-Jozsa 1992 DOI: https://doi.org/10.1098/rspa.1992.0167
- Deutsch 1985 DOI: https://doi.org/10.1098/rspa.1985.0070
- Deutsch full-text mirror: https://docslib.org/doc/7169585/quantum-theory-the-church-turing-principle-and-the-universal-quantum-computer

## Verdict

Minor issues. The mathematics is correct, but source attribution should distinguish original and modern formulations.

## Line-Anchored Comments

- Line 2: source line

  Good to cite Deutsch-Jozsa and Cleve-Ekert-Macchiavello-Mosca for the modern clean formulation. The trick is already implicit in Deutsch's original algorithm, so include Deutsch 1985 as a historical source if this card is meant to be foundational.

- Lines 16 and 20: connection to phase estimation

  Correct in spirit. More precise wording: controlled-`U` acting on an eigenstate writes the eigenvalue phase into the control register after Fourier analysis; it is not exactly the same as converting a Boolean bit oracle into `(-1)^{f(x)}`.

- Line 24: "Zero overhead"

  Query overhead is zero, but circuit overhead is one ancilla plus preparation of `|->`. Say "no additional oracle calls" rather than "zero overhead."

- Line 27: "Only works when f(x) in {0,1}"

  Correct for this Boolean form. Mention the standard qudit/Fourier-state generalization for functions into finite abelian groups.
