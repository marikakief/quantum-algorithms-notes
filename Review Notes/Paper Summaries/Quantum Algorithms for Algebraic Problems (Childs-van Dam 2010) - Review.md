# Review: Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010)

Source note: [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]

## Primary Sources Checked

- Childs and van Dam, "Quantum algorithms for algebraic problems", arXiv:0812.0380 / Reviews of Modern Physics 82 (2010).

## Verdict

Major issues. The note is a useful survey summary, but it overstates at least one result: Watrous does not give a polynomial-time algorithm for arbitrary HSP over all solvable groups. Several broad claims should be narrowed to match the specific algebraic problems and subgroup promises in the cited papers.

## Line-Anchored Comments

- Lines 11-22: good description of the survey's scope.
- Lines 26-31: the organizing theme is right, but "most known exponential speedups" should be softened. There are important exponential speedups outside HSP, and the note later acknowledges quantum walks.
- Lines 35-49: abelian HSP recipe is accurate.
- Lines 52-58: the Hallgren/Pell bullet is useful. Be careful with "group is `R`" language; Hallgren uses a discretized approximation to a real-period function, not a literal finite abelian HSP.
- Lines 60-63: class group/unit group comments should distinguish Hallgren 2002 from later Hallgren/Schmidt-Vollmer work.
- Lines 68-75: the nonabelian overview is mostly right. The phrase "Solvable groups ... HSP over any solvable group" is too broad.
- Line 105: this is the largest error. Replace with something like: "Watrous gives polynomial-time quantum algorithms for key black-box solvable-group tasks such as order and membership, and related tools used in some normal-HSP settings." Do not claim arbitrary solvable-group HSP is solved.
- Line 111: "class groups and unit groups of number fields are all solvable" needs qualifiers about fixed/arbitrary degree and the specific Hallgren/Schmidt-Vollmer algorithms.
- Lines 130-138: caveats are good, but add a caveat about survey compression causing overbroad statements.
- Lines 144-150: the reusable "dichotomy" is useful but should not imply a trichotomy theorem.
- Lines 163 and 196: update the Watrous cross-link label; it is not "non-abelian HSP for solvable groups" in full generality.

## Missing or Suggested Additions

- Add a table separating: abelian HSP, normal HSP in certain groups, solvable black-box group structural tasks, and arbitrary nonabelian HSP. The current prose blends these categories.

