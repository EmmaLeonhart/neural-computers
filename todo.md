# Todo — long-term horizon (neural-computers research)

Abstract destinations, not steps. When work begins on one, it gets decomposed
into concrete `queue.md` items, mirrored to the task tool, and executed. Grounded
in [`literature/REVIEW.md`](literature/REVIEW.md) §6-7 (the gap and what this
project adds).

## The thesis under test

- **The inspectable-substrate bet.** The dominant neural computer (the attention
  stack) is powerful but opaque; interpretability is stuck between unfaithful
  post-hoc recovery and accuracy-taxed by-construction designs (REVIEW §5). The
  bet: a neural computer whose **program is its own execution trace** — built from
  VSA-style legible operations compiled into a single differentiable tensor graph
  — would be *read, not reconstructed*, dissolving the faithfulness gap by
  construction. The open question is whether such a substrate can carry real,
  general-purpose computation against the standing negative results (REVIEW §4:
  DNCs stalled; discrete solvers beat differentiable program induction).

## Experiments (decompose into queue.md when started)

- **E1 — Expressivity vs. legibility.** How much general-purpose computation can a
  by-construction-interpretable VSA/tensor substrate carry before it either loses
  legibility or loses to an opaque model on the same task? Pick a small ladder of
  algorithmic tasks (copy / associative recall / graph traversal — the NTM/DNC
  benchmark set) and measure both task performance AND a legibility metric.
- **E2 — Capacity in superposition.** Where do crosstalk / factorization limits
  (the VSA open problem, REVIEW §3) actually bind for computer-like workloads, and
  do resonator-style decoders extend the usable regime? Produce a capacity curve.
- **E3 — Faithfulness: by-construction vs. recovered.** On a task where both exist,
  does reading the trace of a legible substrate give explanations that post-hoc
  SAE / circuit methods measurably miss or get wrong? Needs a faithfulness metric
  and a baseline opaque model to interpret post-hoc.
- **E4 — The honest comparison.** Against the TerpreT verdict (REVIEW §4), where —
  if anywhere — does a differentiable, legible substrate beat a discrete solver or
  an opaque Transformer on a real task, rather than merely matching them? Report
  the losses plainly, not just the wins.

## The report

- **Eventual shape.** `FINDINGS.md` + the themed `docs/` site: question → what the
  literature establishes → the four experiments and their results (including
  negative ones) → a defensible answer to "can a neural computer be interpretable
  by construction without paying for it." Keep it current as results land.

## Connections (out of scope here, noted for context)

- This question is adjacent to Emma's Sutra (geometric tensor language) and the
  interpretable-OS-prototype work, which motivated the inspectable-substrate
  framing. Those repos are NOT dependencies of this study; this project stands as
  its own investigation and should reach its conclusion on public evidence.
