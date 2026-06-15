# Review: Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016)

Source note: [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes]]

## Verdict

Minor-to-major issues. The note is a useful high-level pointer, but it is too terse for a paper summary. It needs authors, exact feasibility conditions, and a clearer boundary between composite-pulse theory and later QSP/QSVT language.

## Comments

- Add the authors to the metadata. The filename has them, but the note's paper header does not.

- "Completely characterizes" is fair only if the parity, degree, reality, and unitarity/sum-of-squares conditions are stated. The note currently gestures at them without enough detail for audit.

- "Shortest phase sequence" should be qualified: shortest for the realized polynomial degree/response under the equiangular model, not a universal lower bound over all possible pulse families.

- The relationship to QSP is important but retrospective. Avoid implying that QSVT is already present in this paper; the paper is a direct precursor in single-qubit composite-gate language.

- The reusable-ideas list is good, but the actual synthesis algorithm deserves at least a few steps: feasible polynomial tuple -> completion -> recursive phase extraction.

## Suggested Additions

- Add a small theorem box with the polynomial tuple conditions.
- Add one worked example of a target response and how the phase sequence is extracted.
