# Review: Elucidating Reaction Mechanisms on Quantum Computers (Reiher-Wiebe-Svore-Wecker-Troyer 2017)

Source note: [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/1605.03590
- PNAS DOI linked in the source note.

## Verdict

Minor-to-major issues. The note is excellent as a contextual resource-estimation narrative, but several numerical statements mix different accuracy/parallelism scenarios and one algorithmic comparison is tautological. The student should preserve the benchmark story while making the resource table internally consistent.

## Line-Anchored Comments

- Lines 19-21: The opening numbers mix scenarios. The note mentions `~10^14` T gates and 130 days, but the later 0.1 mHa serial table gives `1.1 x 10^15` T gates and 130 days, while `~10^14` corresponds to the optimistic Trotter-rescaling scenario. Separate "main-table quantitative", "qualitative", and "optimistic-rescaled" estimates.

- Lines 29-36: Good workflow summary. Keep the point that the quantum computer accelerates only the CAS-FCI/QPE step; the chemistry pipeline remains hybrid.

- Lines 75-80: The sign-conditioned/`U` versus `U^\dagger` phase-estimation trick is important. Add the assumptions under which it halves unitary calls; it changes phase sensitivity but not state-preparation or overlap costs.

- Lines 108-129: The resource table is useful. Make sure "T gates" versus "T count" and "logical qubits" versus physical qubits are not conflated with Toffoli counts in later timeline rows.

- Lines 135-144: Excellent inclusion of the Trotter-number scenarios. This should be highlighted earlier because the main reported estimate depends on rescaling a loose bound using small-molecule evidence.

- Lines 154-164: The modern timeline is valuable but current-looking. Add citations/status for the 2024-2025 rows or label it as "vault timeline" rather than source-paper content.

- Line 170: The phrase "achieving Heisenberg-limited precision (`O(1/epsilon)` instead of `O(1/epsilon)` with a much better constant)" is tautological. The real contrast is not just the asymptotic `1/epsilon` in QPE, but removal of Trotter step-size/rotation-synthesis overhead and a much smaller block-encoding normalization/oracle cost.

- Lines 200-205: Good caveats. The 100 MHz physical gate assumption should be tied to the original paper's hardware model; modern surface-code cycle assumptions vary widely by architecture.

## Missing or Suggested Additions

- Add a small "do not cite as current best estimate" warning. This paper is historically central but algorithmically superseded.

- Add state-preparation overlap as a separate resource row: the paper acknowledges it but does not cost it.

