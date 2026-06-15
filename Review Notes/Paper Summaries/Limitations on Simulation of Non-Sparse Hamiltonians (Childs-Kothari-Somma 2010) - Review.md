# Review: Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari 2010)

Source note: [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/0908.4398
- QIC DOI metadata linked in the source note.

## Verdict

Minor issues. This is a strong summary of the norm-gap obstruction. The main fix is metadata: the filename includes Somma, but the paper has only Andrew M. Childs and Robin Kothari as authors. The note already flags this, but the review/index filename should not perpetuate the wrong author list.

## Line-Anchored Comments

- Line 5: Good catch on the author mismatch. This warning should be copied into any paper index or bibliography entry because the file title itself is misleading.

- Lines 17-24: The `abs(H)` versus `H` norm-gap explanation is clear and accurate. It would be useful to mention that this is an entry-oracle limitation, not a statement about all possible structured encodings of dense Hamiltonians.

- Lines 29-33: The lower-bound statements should keep the oracle model explicit. These are black-box/query lower bounds for Hamiltonians presented by entry access; they do not preclude efficient simulation from an LCU, Fourier, tensor, low-rank, or block-encoding description.

- Lines 35-38: The positive result is narrower than "some dense Hamiltonians can be simulated efficiently." It relies on small arboricity plus a phase/gauge-clearing argument that controls the relevant absolute-value norm. Keep both hypotheses attached.

- Lines 39-41: The final lesson is right, but phrase it as "the entry-oracle model hides cancellation structure" rather than "the wrong interface" universally. Entry access is still a valid model for lower bounds; it is just not the right interface for many positive algorithmic results.

## Missing or Suggested Additions

- Add a one-sentence statement of the circulant-Hamiltonian reduction: the hard instance encodes a Boolean function in eigenphases diagonalized by the Fourier transform.

- Add a cross-link to block-encoding notes with a warning that block-encoding efficiency is an assumption, not a consequence of density.

