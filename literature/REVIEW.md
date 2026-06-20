# Literature review: neural computers

> **Question.** Survey the field of *neural computers* — differentiable and
> memory-augmented neural architectures (Neural Turing Machines, Differentiable
> Neural Computers, and successors), neural/differentiable approaches to
> general-purpose and OS-like computation, and where interpretable,
> neuro-symbolic, and vector-symbolic (VSA) approaches fit. What has been built,
> what actually works, the open problems, and how interpretability could change
> the picture.

Source catalog with per-paper notes and citations: [`sources.md`](sources.md).
Produced by an agentic-RAG pass (five parallel web-research agents, June 2026);
citations verified against the original sources, with unverifiable details flagged
there.

---

## 1. What "neural computer" has meant

The phrase names a recurring ambition: make *computation itself* a learnable
object. The canonical move (Graves et al., NTM 2014; DNC, *Nature* 2016) couples a
neural **controller** to an external, content-addressable **memory** through
differentiable read/write attention, so a system with the structure of a von
Neumann computer can be trained end-to-end by gradient descent. The DNC could
learn to traverse graphs and answer structured questions; the NTM could induce
copy, sort, and associative recall and generalize to longer inputs than it was
trained on.

A parallel program tried to learn *programs* rather than memory access: Neural
Programmer-Interpreters (compositional program calls), Neural GPU (a universal,
parallel architecture), Neural Programmer (latent program induction over tables),
and differentiable interpreters for real languages (TerpreT, the ∂4 Forth
machine). Universal Transformers later showed that adding recurrence-in-depth plus
adaptive halting restores Turing-completeness (under idealized assumptions) to the
otherwise fixed-depth Transformer.

## 2. The great convergence: memory became attention

The field's most consequential result is not a new computer but a unification.
Explicit external-memory modules (Memory Networks, End-to-End Memory Networks,
Key-Value Memory, Pointer Networks, Neural Stacks) converged on the realization
that **soft content-based attention is itself an associative-memory read**.
End-to-End Memory Networks' multi-hop soft addressing leads directly into
Transformer self-attention, and "Hopfield Networks is All You Need" (2020) made
the equivalence formal: an attention layer is a single-step content-addressable
retrieval from a modern Hopfield memory.

Once memory *was* attention, "neural computer" memory research became Transformer
memory research, and split into two scalable branches:

- **Non-parametric retrieval** — RAG, RETRO, Memorizing Transformers — treat an
  enormous external corpus or KV cache as editable memory beyond the context
  window.
- **Parametric memory layers** — Product-Key Memory → Memory Layers at Scale
  (128B memory params) — add cheap, sparsely-accessed trainable capacity decoupled
  from compute.

A recent wave (Recurrent Memory Transformer, Titans) returns to *learned*
recurrent write/forget dynamics at test time — the NTM/DNC spirit, re-expressed
inside the dominant architecture.

## 3. The VSA / neuro-symbolic alternative: computing in superposition

A different lineage gives a neural computer an explicit **datatype and instruction
set** rather than a learned memory controller. Vector-symbolic architectures /
hyperdimensional computing (Plate's Holographic Reduced Representations 1995;
Kanerva's hyperdimensional computing 2009; Gayler's VSA framing 2003) represent
everything as fixed-width high-dimensional vectors and compute with three
algebraic primitives — **bind** (associate / key-value), **bundle** (superpose a
set into one vector), **permute** (encode order) — plus a **clean-up memory** to
denoise. Because random hypervectors are quasi-orthogonal and the operations are
noise-robust, a single vector can hold a whole record, sequence, tree, or graph,
and queries are run by unbinding (computing in superposition). Resonator networks
solve the inverse problem of factoring a composite vector back into its parts.

VSA's distinctive properties for a neural computer are **compositionality**,
**transparent (interpretable) operations**, and a natural fit to stochastic,
in-memory, neuromorphic hardware (Kleyko et al., *Proc. IEEE* 2022). Neuro-symbolic
systems (Neuro-Symbolic Concept Learner, DeepProbLog, Logical Neural Networks)
pursue the same "computation made of named, inspectable parts" goal from the logic
side, delivering data-efficient, auditable reasoning over learned perception.

## 4. What actually works, and the negative results

- **Works:** differentiable content-based attention over a store (now the backbone
  of every large model); retrieval and memory layers that scale capacity
  independently of compute; HDC classification that is energy-efficient and
  fault-tolerant; VSA factorization via resonator networks; neuro-symbolic hybrids
  for compositional, data-efficient reasoning.
- **Stalled / failed:** explicit read/write-head computers (NTM/DNC) never escaped
  small synthetic benchmarks — training instability, poor parallelism, and dense
  addressing/linkage that scales badly (the sparse-access and de-allocation papers
  chased exactly these). The bluntest verdict is TerpreT's controlled comparison:
  for actual program synthesis, **discrete constraint/SAT solvers beat
  differentiable relaxations**. "Just make the interpreter differentiable" did not
  win.
- **A category error to avoid:** the recent "LLM-OS" framing (MemGPT, AIOS,
  Karpathy's metaphor) is *not* differentiable computation. The model is frozen and
  non-differentiable in deployment; the "operating system" is orchestration and
  memory-management scaffolding around it. It answers "computer as learnable
  object" oppositely to the classic work: treat an already-trained model as the
  computer and engineer an OS around it.

## 5. The interpretability turn

If the dominant neural computer is a Transformer, can we read its computation?
Two strategies, in tension:

- **Post-hoc recovery** — the circuits program (Olah; Elhage; induction heads),
  sparse-autoencoder feature dictionaries (Towards/Scaling Monosemanticity), and
  2025 attribution-graph circuit tracing — has produced genuinely readable
  artifacts at frontier scale. But it rests on **superposition** making individual
  neurons unreadable, and its workhorse (SAEs) is now shown to fragment and absorb
  features, leave large unexplained reconstruction error, and sometimes lose to
  trivial probing/steering baselines. The recovered "program" is incomplete and
  possibly partly artifactual.
- **Interpretable-by-construction** — Rudin's argument that explanations of black
  boxes are inherently unfaithful, instantiated as concept bottleneck models
  (reasoning routed through named variables) and white-box transformers / CRATE
  (each layer *is* a known optimization step). These pay an accuracy/scale tax and
  constrain what the network may represent.

The unresolved problem is **faithfulness**: a post-hoc explanation can never be
guaranteed to match the underlying computation, and by-construction methods do not
yet scale to frontier performance.

## 6. Synthesis: where interpretable / neuro-symbolic / VSA fit

Three findings organize the field:

1. **The mainstream answer to "neural computer" is the attention/associative-memory
   stack** — memory dissolved into attention, and capacity is now bought with
   retrieval or sparse memory layers. It works, but its computation is opaque and
   read only after the fact, imperfectly.
2. **The explicit "learn the computer" line largely lost** on its own terms
   (instability; discrete solvers beat differentiable program induction). What
   survived from it is the attention primitive, not the read/write-head machine.
3. **VSA/neuro-symbolic is the standing alternative whose operations are
   transparent by construction** — bind/bundle/permute are legible algebra, and
   neuro-symbolic executors expose named symbols and programs. Its open problems
   are capacity/crosstalk, shallow learning versus deep nets, and scale.

The gap sits at the intersection of (1) and (3): the dominant neural computer is
powerful but unreadable, and interpretability is stuck choosing between unfaithful
post-hoc recovery and an accuracy-taxed by-construction design. VSA gives a
substrate whose operations are inherently legible, but it has not been shown to
carry general-purpose, scalable computation.

## 7. What this project adds

The opening this review surfaces is an **inspectable substrate**: a neural computer
where the program *is* its own execution trace, so its computation is **read, not
reconstructed**. That dissolves the faithfulness gap of Section 5 by construction —
there is no recovered approximation that can be wrong, because the readable
structure and the running computation are the same object — while inheriting VSA's
transparent, compositional operations (Section 3) rather than the opaque attention
stack (Section 2). The bet this project investigates is whether such a substrate
(VSA-style operations compiled into a single differentiable tensor graph that
remains legible) can carry real, general-purpose computation, against the standing
negative result that differentiable computation has historically lost to both
opaque attention models and discrete solvers (Section 4).

Concretely, the questions this implies and that the project will decompose into
`todo.md`:

- **Expressivity vs. legibility.** How much general-purpose computation can a
  by-construction-interpretable VSA/tensor substrate carry before it either loses
  legibility or loses to an opaque model on the same task?
- **Capacity in superposition.** Where do crosstalk/factorization limits (the VSA
  open problem) actually bind for computer-like workloads, and do resonator-style
  decoders extend the usable regime?
- **Faithfulness by construction vs. recovered.** On a task where both exist, does a
  read-the-trace substrate give explanations that post-hoc SAE/circuit methods
  measurably miss?
- **The honest comparison.** Against the TerpreT verdict, where (if anywhere) does a
  differentiable, legible substrate beat a discrete solver or an opaque
  Transformer, rather than merely matching them?

The contribution is to locate that thesis precisely against what has been tried:
not to re-litigate the differentiable-computer dream, but to test whether
*interpretability-by-construction*, not raw capability, is the axis on which a
neural computer can earn its place.

---

*Grounded in the sources cataloged in [`sources.md`](sources.md). Where a citation
detail could not be verified during the research pass, it is flagged there;
treat those as provisional until checked against the primary source.*
