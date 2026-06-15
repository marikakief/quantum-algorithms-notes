# Review: Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999)

Source note: [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes]]

## Primary Sources Checked

- Nayak, "Optimal lower bounds for quantum automata and random access codes", FOCS 1999 / arXiv:quant-ph/9904093.

## Verdict

Minor issues. The entropy-accumulation proof is well captured. The main fixes are a mislabeled table row, a few model qualifiers, and a top-level heading.

## Line-Anchored Comments

- Add a top-level title.
- Lines 13-23: good summary of the two headline results.
- Lines 25-33: the Holevo/Fano lemma is clear. It would be useful to say logs are base 2 when writing `1-H(p)` bits.
- Lines 35-49: the QRAC induction is correct and nicely presented.
- Line 49: the serial-code extension is important. Keep it, but define "serial" before using it if this becomes a standalone note.
- Lines 51-65: the QFA entropy-growth argument is good. "Measurements can only increase entropy" should be understood as the nonselective measurement channel used here; selective post-measurement branches need separate handling.
- Lines 79-85: the row "Corollary: non-regular languages" is mislabeled. The statement is about enhanced one-way QFAs recognizing only a strict subset of regular languages, not about recognizing non-regular languages.
- Line 99: the average-case caveat needs precision. Average over indices, messages, or both can lead to different RAC notions; do not leave it as a broad "better compression may be possible" claim.

## Missing or Suggested Additions

- Add a short comparison with modern random-access-code terminology: worst-case bit recovery, average-case recovery, entanglement-assisted variants.

