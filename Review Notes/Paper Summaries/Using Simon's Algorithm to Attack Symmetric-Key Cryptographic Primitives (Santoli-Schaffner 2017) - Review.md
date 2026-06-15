# Review: Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017)

Source note: [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes]]

## Primary Sources Checked

- Santoli and Schaffner, "Using Simon's algorithm to attack symmetric-key cryptographic primitives," arXiv:1603.07856v3.

## Verdict

Minor issues. The note is strong and correctly reads the result as a Q2 model-separation paper rather than an implementation threat. The weak-period Simon observation is explained especially well.

## Line-Anchored Comments

- Lines 11-15: good oracle model. Coherent superposition access is the whole point and must not be buried.
- Lines 37-48: the CBC-MAC security notion is stated clearly. The difference from Boneh-Zhandry's standard unqueried-in-superposition notion should remain in the comparison section.
- Lines 58-72: excellent weak-period explanation. This is the technical reason the Feistel distinguisher survives extra collisions.
- Lines 76-121: the Feistel distinguisher is accurate. The "rank failed" branch returning Feistel is deliberate and does not create false negatives in the Feistel case.
- Lines 123-150: the CBC-MAC construction is well described. The exact Simon promise for each `g_j` depends on the block primitive being a permutation.
- Lines 152-179: the alternating-state proof for the forgery is clear. Keep the parity of `ell` in the tag formula because it is easy to misquote.
- Lines 211-227: the result statement is good. The coherent query count `O(ell n)` plus two classical queries is the right level of detail.
- Lines 231-239: comparison with Boneh-Zhandry is important. The forgery notion differs, so this should not be stated as a direct contradiction of Boneh-Zhandry security.
- Lines 241-247: caveats are good. The paper separates three-round Feistel only; do not generalize to all constant-round Feistel networks.

## Missing or Suggested Additions

- Add a small "Q2 only" status label near the title-level summary.
- Cross-link to Kaplan--Leurent--Leverrier--Naya-Plasencia as the complementary accounting paper for non-Simon symmetric attacks.

