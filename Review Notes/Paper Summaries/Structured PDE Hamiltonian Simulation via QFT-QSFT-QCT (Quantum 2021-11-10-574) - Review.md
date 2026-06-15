# Review: Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574)

Source note: [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574)]]

## Verdict

Minor-to-major issues. The note is too short and the filename/title appears mismatched with the paper it summarizes. The underlying idea may be right, but the current note is not sufficiently reliable as a paper summary.

## Comments

- The first problem is bibliographic: identify the paper by authors and title in the note header. "Quantum 2021-11-10-574" is not enough for later audit.

- If this is Childs-Liu-Ostrander's high-precision PDE algorithm, the note should say so explicitly and use that title rather than a homemade QFT-QSFT-QCT title.

- The runtime slogan "poly(d, log 1/epsilon)" hides too much. PDE quantum algorithms depend on smoothness, dimension, discretization, condition number, norm bounds, boundary conditions, and output model.

- The role of the QFT, quantum sine/cosine transforms, and block encodings should be explained structurally. Right now the transforms are named but not connected to the differential operator being diagonalized.

- State preparation and readout are the usual bottlenecks. The algorithm prepares or manipulates a solution state; it does not output a full grid solution classically in polylogarithmic time.

## Suggested Additions

- Rewrite the note with standard metadata: authors, exact title, arXiv/DOI, problem class, assumptions, output model, and theorem.
- Add a comparison to FEM/QLSA notes, since the same condition-number and readout caveats recur.
