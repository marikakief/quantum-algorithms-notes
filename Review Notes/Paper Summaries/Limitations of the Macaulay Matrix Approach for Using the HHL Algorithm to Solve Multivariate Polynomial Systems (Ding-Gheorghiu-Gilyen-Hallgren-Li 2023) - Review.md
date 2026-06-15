# Review: Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023)

Source note: [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]

## Verdict

Minor issues. The note has the right message: the HHL/Macaulay route has a narrow and highly conditioned advantage regime. The main fixes are typographical and scope-related.

## Comments

- The source note contains a malformed control character in one displayed formula. That should be cleaned in the original note, because it will break search/rendering and possibly Obsidian export.

- The limitation is not a theorem that "HHL cannot solve polynomial systems." It is a limitation of the Macaulay-matrix embedding under the paper's assumptions, especially condition-number and solution-structure constraints.

- Keep the convex-hull/equal-weight solution condition visible. The negative result depends on the geometry of the solution set and the output state one hopes HHL will produce.

- The comparison to classical algebraic solvers should separate exact symbolic solving, numerical homotopy methods, and decision/search variants over finite fields. They are not interchangeable baselines.

- The unfinished cross-links are worth fixing because this note is part of the vault's recurring HHL caveat theme.

## Suggested Additions

- Add a compact pipeline diagram: polynomial system -> Macaulay matrix -> linear-system solution state -> attempt to extract root information.
- Add a "what survives" paragraph identifying any regimes where Macaulay/HHL may still be useful.
