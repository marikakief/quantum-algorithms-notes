
> **Tags:** #trick #state-preparation #tda #combinatorics
> **Source:** Babbush et al. arXiv:2209.13581, "Analyzing Prospects for Quantum Advantage in Topological Data Analysis" (PRX Quantum 2024)

## What it does
Efficiently prepares uniform superposition over Hamming-weight-$k$ bitstrings (Dicke state), used to represent $k$-simplices.

## The trick
Instead of expensive recursive exact synthesis, [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] a candidate superposition and use inequality/comparator logic to enforce Hamming-weight constraints, then [[Oblivious Amplitude Amplification (Robust)|amplitude amplification]] to boost success.

Target state:
$$|D_{k,n}\rangle = \frac{1}{\sqrt{\binom{n}{k}}}\sum_{x:|x|=k}|x\rangle$$

In TDA, each basis string encodes a $k$-subset of vertices (candidate simplex).

## When to reach for it
- Uniform sampling over fixed-size subsets
- Clique/simplex superpositions (TDA, combinatorial search)
- Any algorithm with symmetry over $k$-combinations

## Complexity
Poly($n$, $\log(1/\varepsilon)$) state-prep circuit with improved constants relative to earlier TDA constructions (e.g., the original Lloyd–Garnerone–Zanardi 2016 proposal). The amplitude amplification overhead depends on the ratio $\binom{n}{k}/2^n$ — for $k = O(1)$ this is fine; for $k \sim n/2$, acceptance amplitude is $\Omega(1)$ so AA is not needed.

Note: Dicke state preparation independent of this paper has been studied extensively; the inequality-testing approach here is specifically tailored for the simplicial complex context.

## Caveat
Need a good bound on acceptance amplitude for AA efficiency. If valid subset ratio is tiny, prep can still be costly.
