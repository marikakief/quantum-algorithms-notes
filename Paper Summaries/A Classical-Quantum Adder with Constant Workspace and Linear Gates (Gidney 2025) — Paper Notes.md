> **Source:** Craig Gidney, *A Classical-Quantum Adder with Constant Workspace and Linear Gates*, arXiv:2507.23079 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2507.23079)
> **Tags:** #quantum-arithmetic #T-complexity #fault-tolerant #Toffoli #adder #circuit-primitive #classical-quantum

---

## The computational problem

Add a classically known $n$-bit offset $d$ into an $n$-qubit register $|x\rangle$ in place:

$$|x\rangle \mapsto |x + d + c_0 \bmod 2^n\rangle$$

where $c_0$ is a carry-in bit. The offset $d$ is a classical constant known at circuit-generation time, so its bits are not stored in qubits. The goal is to minimise Toffoli count and ancilla usage simultaneously — ideally $O(n)$ Toffolis with $O(1)$ clean ancillae.

This is the classical-quantum (CQ) variant of addition. The quantum-quantum (QQ) variant — adding two quantum registers — was solved with $O(n)$ gates and $O(1)$ ancillae by [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro et al. (2004)]]. But their technique stores carry information in the offset register, which doesn't exist when the offset is classical. The CQ case has been open since 2004.

## What the paper does

Closes a 21-year-old open problem by constructing a CQ adder with $4n \pm O(1)$ Toffolis and only 3 clean ancillae — matching the asymptotic gate count of the QQ adder while using constant workspace. Also presents a variant with $3n \pm O(1)$ Toffolis when $n - 2$ dirty ancillae are available. Both variants can be controlled by an additional qubit at zero extra Toffoli cost.

The construction rests on three ideas: (1) "venting" carry qubits via X-basis measurement to convert garbage into phasing tasks, (2) resolving those phasing tasks using a carry-xor circuit from [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner, Roetteler, Svore (2017)]], and (3) splitting the addition into two halves that borrow each other as dirty workspace.

## The algorithm / construction

### Idea 1: Streaming addition by venting carries

In a ripple-carry addition, carry bit $c_{k+1} = \text{maj}(x_k, d_k, c_k)$ depends only on the current bit position and the previous carry. Once $c_{k+1}$ is computed and the sum bit $x'_k = x_k \oplus d_k \oplus c_k$ is written, the old carry $c_k$ becomes garbage.

A key observation: the carries of $x + d + c_0$ are exactly reproduced by adding the same offset into the bitwise complement of the result:

$$\text{carry}(\sim x', d, c_0) = \text{carry}(x, d, c_0)$$

This means each carry qubit is a Z-basis function of values available *after* the addition completes — Gidney calls this "Z-redundant." A Z-redundant qubit can be erased by measuring it in the X basis ([[Measurement-Asymmetric Uncomputation|measurement-based uncomputation]]). The $|+\rangle$ outcome means clean erasure; the $|-\rangle$ outcome means an unwanted phase flip $Z_{c_k}$ occurred just before deletion.

The process of converting carry garbage into phasing tasks via X-basis measurement is called **venting**. A vented adder uses just 2 clean ancillae (recycled for each carry) and $n \pm O(1)$ Toffolis, but leaves behind $n - 2$ unresolved phasing tasks.

### Idea 2: Phasing by carry-xor

[[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner et al.]] described a carry-xor circuit that XORs the carries of a planned addition into a secondary register: $g \to g \oplus \text{carry}(x, d, c_0)$. This costs $n \pm O(1)$ Toffolis and requires $n - 2$ dirty target qubits.

The connection to venting: interleaving phase flips with controlled bit flips transfers the phase onto the control. So interleaving classically conditioned $Z$ gates (from the venting measurement outcomes) with carry-xor operations resolves the phasing tasks. Two carry-xor passes are needed (to return the dirty register to its original state), but the first can be merged into the streaming adder cheaply.

This gives a CQ adder using $3n \pm O(1)$ Toffolis, 2 clean ancillae, and $n - 2$ dirty ancillae.

### Idea 3: Borrowing back and forth

To eliminate the need for $n - 2$ external dirty ancillae, split the target register into two halves ($n/2$ bits each). Each half borrows the other as dirty workspace:

1. **Vented addition into the bottom half** ($n/2$ bits). Compute the carry-out qubit needed by the top half. No carry-xor yet (the top half is needed as workspace later).
2. **Vented addition into the top half** with a merged carry-xor, borrowing the bottom half as dirty target.
3. **Fix top-half phases** by targeting the bottom half with a carry-xor conjugated by classically controlled $Z$ gates.
4. **Vent the bottom-to-top carry** qubit (now unneeded).
5. **Fix bottom-half phases** using two carry-xors targeting the (now-available) top half.

Total: $4n \pm O(1)$ Toffolis ($n \pm O(1)$ for streaming additions, $3n \pm O(1)$ for carry-xors) and 3 clean ancillae (two for the vented additions, one for the inter-half carry).

### Free control

All the adders in this paper can be controlled at zero additional Toffoli cost. Since each classical bit $d_k$ only appears as a control inversion or a classical bit-flip control, substituting $d_k = 1 \to \text{control qubit}$ and $d_k = 0 \to 0$ introduces no new Toffoli gates.

## Key results

**CQ adder (constant workspace):**
$$\text{Toffolis} = 4n \pm O(1), \quad \text{clean ancillae} = 3$$

**CQ adder (dirty workspace):**
$$\text{Toffolis} = 3n \pm O(1), \quad \text{clean ancillae} = 2, \quad \text{dirty ancillae} = n - 2$$

**Controlled variants:** Same Toffoli count and workspace as uncontrolled.

## Comparison with prior work

| Construction | Toffolis | Clean ancillae | Dirty ancillae | Notes |
|---|---|---|---|---|
| Cuccaro (2004) QQ adder | $2n + O(1)$ | 1 | 0 | Stores carries in offset register — CQ impossible |
| [[Dirty Ancilla Constant Addition\|Häner et al. (2017)]] CQ adder | $O(n \log n)$ | 0 | 1+ | Divide-and-conquer; best prior CQ result |
| Gidney (2015) CQ incrementer | $O(n)$ | $O(1)$ | 0 | Special case $d = 1$ only |
| [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes\|Gidney (2018)]] QQ adder | $4n + O(1)$ T gates | $n - 1$ | 0 | [[Temporary Logical-AND]]; T-count not Toffoli count |
| **Gidney (2025) — this paper** | $3n \pm O(1)$ | 2 | $n - 2$ | Dirty workspace variant |
| **Gidney (2025) — this paper** | $4n \pm O(1)$ | 3 | 0 | Constant workspace variant |

This paper closes the asymptotic gap between CQ and QQ adders. The $O(\log n)$ factor from [[Dirty Ancilla Constant Addition|Häner et al.'s divide-and-conquer]] is eliminated entirely.

## Limits / caveats

1. **Linear depth.** Both variants have $O(n)$ depth. Constant workspace rules out parallelism — parallel operations require workspace proportional to the parallelism for routing and magic state production. Low-depth tradeoffs remain open.

2. **No quantum carry-save adder.** The paper's conclusion notes that a "true" quantum carry-save adder (accumulating offsets with $O(1)$ marginal depth and $O(n)$ storage) does not yet exist. All known attempts have $\Omega(\log n)$ or $\Omega(\log \log 1/\varepsilon)$ multiplicative overhead.

3. **Measurement and feedforward.** The venting step requires mid-circuit X-basis measurement and classical feedforward of the measurement outcomes to determine phase corrections. Natural in the surface code; may add latency in other architectures.

4. **Toffoli vs T count.** The paper counts Toffolis, not T gates. Converting to T-count depends on the Toffoli implementation. Using [[Temporary Logical-AND]], each Toffoli in a compute/uncompute pair costs 4 T gates. But the Toffolis here do not all appear in neat pairs — some are in the carry-xor circuits, which have their own structure.

## Reusable ideas

1. [[Carry Venting via X-Basis Measurement]] — Convert carry garbage into phasing tasks by measuring carry qubits in the X basis, exploiting the Z-redundancy of carries (the complement-addition identity). Eliminates the need to store or uncompute carry qubits.

2. [[Half-Register Dirty Workspace Borrowing]] — Split a register into two halves and use each half as dirty workspace for the other. Eliminates external ancilla requirements at the cost of sequential processing.

## References within this paper

- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro, Draper, Kutin, Moulton (2004)]] — the QQ adder whose $O(n)$-gate, $O(1)$-ancilla result this paper extends to the CQ case
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner, Roetteler, Svore (2017)]] — carry-xor construction and the prior $O(n \log n)$ CQ adder; key building block
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] — QFT-based adder (mentioned in the conclusion re: carry-save)
- Gidney (2015) — blog post on the constant-workspace incrementer ($d = 1$ special case); [algassert.com](https://algassert.com/circuits/2015/06/12/Constructing-Large-Increment-Gates.html)
- Gidney (2019) — "Spooky Pebble Games and Irreversible Uncomputation"; formalises the venting/[[Measurement-Asymmetric Uncomputation|measurement-based uncomputation]] technique used throughout
- Vedral, Barenco, Ekert (1996) — first quantum adder circuits with $\Theta(n)$ gates and $\Theta(n)$ workspace
- Gossett (1998) — quantum carry-save adder attempt; arXiv:quant-ph/9808061
- Kim et al. (2025) — tree-based quantum carry-save adder; another approach to the open carry-save problem

## Cross-links

### Paper notes
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — Gidney's earlier adder paper; introduced [[Temporary Logical-AND]] and [[Measurement-Asymmetric Uncomputation]]; this paper builds on both ideas
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — the QQ adder whose CQ analogue was the open problem
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — the QFT-based alternative that avoids carry qubits entirely
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — provides the carry-xor circuit and the prior CQ adder
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — major application of Gidney's arithmetic primitives in quantum chemistry

### Trick cards
- [[Carry Venting via X-Basis Measurement]] — new, from this paper
- [[Half-Register Dirty Workspace Borrowing]] — new, from this paper
- [[Temporary Logical-AND]] — predecessor technique from [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]]
- [[Measurement-Asymmetric Uncomputation]] — the underlying principle behind venting
- [[Dirty Ancilla Constant Addition]] — the prior CQ adder technique this paper supersedes
- [[In-Place Majority for Carry Propagation]] — the MAJ gate used in carry computation
- [[MAJ-UMA Decomposition for Reversible Adders]] — the paired structure from Cuccaro's adder
- [[Ancilla Opportunity Cost Analysis]] — relevant to the workspace tradeoff between the two variants
