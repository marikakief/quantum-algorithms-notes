# Review: A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021)

Source note: [[A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes]]

## Primary Sources Checked

- Harvey and Hittmeir, "A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation," arXiv:2105.11105 (2021).

## Verdict

Minor issues. The note is careful and unusually concrete for a classical number-theory algorithm. The main improvements are editorial: keep the "not a quantum algorithm" status visible, and avoid letting the clean parameter story hide the asymptotic/large-constant nature of the result.

## Line-Anchored Comments

- Lines 11-21: good framing. Since this vault is quantum-algorithm focused, repeat the line "classical factoring, not a quantum algorithm" in the one-line summary or title block.
- Lines 13-14 and 37-39: the reduction to prime-or-semiprime input is right in spirit, but the `N^{1/6+o(1)}` statement depends on using a deterministic small-factor search subroutine, not naive trial division. Add the algorithmic source of this reduction.
- Lines 25-31: "technically neat, but incremental" is fair, but slightly undersells the proof value. The exponent is unchanged, but the paper is a rigorous asymptotic improvement in the deterministic factoring frontier.
- Lines 41-62: the large-order subroutine is described well. It would help to state explicitly how the later order condition for `beta^{m^2}` follows from the chosen parameters.
- Lines 82-88: the Mertens-density explanation is clear and likely the most useful part of the note.
- Lines 90-124: strong account of the rank-2 lattice step. Mention that the lattice step is a coefficient-construction tool, not the source of the asymptotic bottleneck.
- Lines 165-171: the product-tree/Bluestein/GCD collision mechanism is good. This is worth keeping because it explains why a collision modulo an unknown factor can still be found.
- Lines 242-250: the comparison table is useful. Keep Shor clearly marked as a different computational model, not a point on the same deterministic-classical curve.
- Lines 281-284: the `Ekerå-Håstad` cross-link appears to use decomposed diacritics and may not match the vault filename. Check that Obsidian resolves it.

## Missing or Suggested Additions

- Add a short "why this belongs here" status box: classical baseline near Shor-style factoring, not a quantum speedup paper.
- Add the dependency chain for the small-factor and large-order subroutines so readers can see which older deterministic factoring results are being imported.

