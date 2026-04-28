# Quantum Oracle Sketching

> **Source:** Zhao, Zlokapa, Neven, Babbush, Preskill, McClean, Huang, arXiv:2604.07639
> **Tags:** #trick #streaming #oracle-models #query-complexity #quantum-advantage

## What it does

Builds a usable quantum oracle directly from streaming classical samples, without storing the full dataset or assuming qRAM.

## The trick

Suppose samples arrive as $(x, f(x))$ with $x \sim p$, and the downstream quantum algorithm wants coherent access to a phase oracle like
$$
U = \sum_x e^{i p(x) f(x) t} |x\rangle\langle x|.
$$

Instead of first reconstructing $f$ classically and then compiling $U$, process each sample once and apply a tiny phase update whose product over the stream approximates $U$ in expectation.

The key point is compositionality: if a later quantum algorithm would use the oracle $Q$ times, one only needs to instantiate the oracle accurately enough for those $Q$ uses, and the required number of classical samples scales quadratically in $Q$.

For sparse matrices and vectors, this basic move gets wrapped into larger constructions that produce sparse oracles, block encodings, and even state-preparation unitaries from streamed entries.

## When to reach for it

- When the natural input model is a data stream rather than random-access memory
- When qRAM assumptions would otherwise hide the hard part of the algorithm
- When you want a quantum advantage statement that explicitly pays for data loading
- When the downstream quantum routine is query-based, so its query complexity can be converted into sample complexity

## Complexity

In the paper's model, if the downstream algorithm uses $Q$ oracle queries, oracle sketching needs sample complexity roughly
$$
M \sim N Q^2
$$
up to logs and model-dependent factors such as repetition number $R$, precision, and distribution parameters.

A central lower bound in the paper shows this $Q^2$ dependence is sample-optimal up to logarithmic factors.

## Caveat

This does **not** give free state/input access. You still pay at least linear-in-data sample cost, and often more. The trick is about replacing impossible full-memory storage by a coherent incremental construction, not about making loading disappear.

Also, the guarantees are strongest in a specific bounded-space streaming model. Outside that model, classical algorithms may exploit different access patterns.

## Related notes
- [[Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) — Paper Notes]]
- [[Interferometric Classical Shadow]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Forrelation as Quantum-Classical Separator]]
