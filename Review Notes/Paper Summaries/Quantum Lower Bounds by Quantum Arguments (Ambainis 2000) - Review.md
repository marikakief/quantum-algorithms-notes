# Review: Quantum Lower Bounds by Quantum Arguments (Ambainis 2000)

Source note: [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]

## Primary Sources Checked

- Ambainis, "Quantum lower bounds by quantum arguments", STOC 2000 / JCSS 2002.

## Verdict

Major issues. The main adversary-method summary is strong, but the note contradicts itself on element distinctness. The caveat correctly says the positive adversary method cannot prove the tight element-distinctness lower bound; the cross-link later says it does.

## Line-Anchored Comments

- Add a top-level title. The note starts with metadata rather than an H1.
- Lines 15-23: good overview of the quantum adversary idea and its historical role.
- Lines 55-81: the parameter version of the adversary method is clear. It would help to name this as the positive-weight/unweighted Ambainis method to distinguish it from the later general adversary bound.
- Lines 89-97: the search and AND-OR examples are appropriate.
- Lines 125-129: this caveat is important and correct for the positive adversary method and its positive-weight variants. The certificate-complexity barrier blocks an `Omega(N^{2/3})` element-distinctness lower bound.
- Line 157: "Nayak & Wu ... proved here by adversary method" should be checked. If this means Ambainis reproves or derives approximate-median lower bounds using his method, say that explicitly; otherwise it reads as if Nayak-Wu's original polynomial-method result was adversary-based.
- Line 169: this is wrong. The tight `Omega(N^{2/3})` element-distinctness lower bound is due to Aaronson-Shi via the polynomial method, not to the Ambainis 2000 positive adversary method. The corresponding Ambainis 2007 paper is an upper-bound quantum walk algorithm. Replace with: "The positive adversary method cannot prove the tight lower bound; later general/negative adversary methods escape this limitation."

## Missing or Suggested Additions

- Add a short timeline: Ambainis 2000 positive adversary, Ambainis 2003 weighted positive adversary, Hoyer-Lee-Spalek negative adversary, Reichardt tightness via span programs. This will prevent the cross-links from mixing eras.

