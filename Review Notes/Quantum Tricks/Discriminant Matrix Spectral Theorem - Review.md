# Review: Discriminant Matrix Spectral Theorem

Source note: [[Discriminant Matrix Spectral Theorem]]

## Primary Sources Checked

- Szegedy, "Quantum speed-up of Markov chain based algorithms", FOCS 2004 / arXiv version checked previously in tranche 4.
- Krovi-Magniez-Ozols-Roland and Childs 2010 checked as downstream uses of the spectral mapping.

## Verdict

Minor issues. The card contains the right idea, but its matrix notation is nonstandard enough that it could confuse readers. It should distinguish Szegedy's discriminant matrix from a generic Gram-minus-identity matrix and state the singular-value/cosine interpretation.

## Line-Anchored Comments

- Lines 11-17: discriminant definition is too generic.

  In Szegedy walks for Markov chains, the discriminant matrix has entries `sqrt(P_xy P_yx)` or, equivalently, overlaps between two families of walk states. Calling it `Gram(...) - I` is a valid abstraction for two subspaces, but it hides the Markov-chain content.

- Lines 19-25: eigenvalue formula needs convention labels.

  Product-of-reflections spectra are usually stated in terms of principal angles or singular values of the overlap matrix. The displayed `2 lambda^2 - 1` form is one convention. Other notes use `cos theta = lambda`. Add a sentence reconciling these conventions.

- Line 31: "folds [-1,1]"

  Since the theorem uses singular values/principal cosines in `[0,1]`, the `[-1,1]` language is potentially misleading unless one first extends to signed eigenvalues of a symmetric discriminant.

- Lines 35-37: qubitization connection is directionally right.

  Make it "structural ancestor" rather than "this same relationship" unless the block-encoding/qubitization conventions are spelled out. Qubitization uses a closely related product-of-reflections spectral mapping, but not necessarily this exact `M` definition.

- Line 47: eigenvector normalization caveat is good.

  This is exactly the kind of technical warning students miss.

## Missing or Suggested Additions

- Add a tiny notation table: overlap singular value `lambda`, principal angle `theta`, walk eigenphase under the chosen reflection order.
