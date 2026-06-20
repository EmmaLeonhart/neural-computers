# Sources — neural computers literature review

Catalog of sources for the survey of **neural computers**: differentiable and
memory-augmented architectures, neural/differentiable approaches to
general-purpose computation, and where interpretable / neuro-symbolic /
vector-symbolic approaches fit. Gathered by an agentic-RAG pass (five parallel
web-research agents, one per sub-area, June 2026). Every citation was checked
against the actual source; items the researchers could not fully verify are
flagged inline. Synthesis lives in [`REVIEW.md`](REVIEW.md).

---

## 1. The Neural Turing Machine / Differentiable Neural Computer lineage

Couple a neural **controller** to an external, content-addressable **memory
matrix** via differentiable **read/write heads**, trainable end-to-end by
gradient descent.

- **Graves, Wayne & Danihelka (2014). "Neural Turing Machines."** arXiv:1410.5401.
  The foundational paper: controller + external memory accessed by differentiable
  attention combining content-based and location-based (rotational) addressing.
  Learned copy, repeat-copy, associative recall, and sorting from examples,
  generalizing to longer sequences than seen in training. Toy algorithmic tasks;
  training notoriously fiddly.
- **Graves, Wayne, Reynolds, Harley, Danihelka, et al. (2016). "Hybrid computing
  using a neural network with dynamic external memory" (Differentiable Neural
  Computer).** *Nature* 538(7626):471-476. DOI:10.1038/nature20101. The flagship.
  Adds dynamic memory allocation (usage vector), a temporal link matrix (write
  order), and content read modes. Answered bAbI-style questions, traversed graphs
  (London Underground, family trees), learned a block-puzzle plan via RL. Shown
  only on small structured tasks; dense access scales poorly; training unstable.
- **Rae, Hunt, Danihelka, Harley, Senior, Wayne, Graves & Lillicrap (2016).
  "Scaling Memory-Augmented Neural Networks with Sparse Reads and Writes."**
  NeurIPS 2016. arXiv:1610.09027. Sparse Access Memory (SAM): restrict reads/writes
  to relevant slots via approximate nearest-neighbor. ~1000x faster, ~3000x less
  memory; Sparse DNC >400x faster than dense DNC at 2,000 slots. Adds ANN-indexing
  complexity and gradient approximation.
- **Csordás & Schmidhuber (2019). "Improving Differentiable Neural Computers
  Through Memory Masking, De-allocation, and Link Distribution Sharpness Control."**
  arXiv:1904.10278 (a related short version was a NeurIPS 2018 workshop paper;
  main-conference venue not fully verified — treat as arXiv/workshop). Diagnoses
  three DNC flaws (no key/value separation, content aliasing after de-allocation,
  link-distribution degradation); fixes cut mean bAbI error 43%. Shows the
  instability was architectural, not fundamental.
- **Yang & Rush (2017). "Lie-Access Neural Turing Machines."** ICLR 2017.
  arXiv:1611.02854. Places memory in a continuous key-space manifold accessed by
  Lie-group actions (shifts/rotations), giving clean differentiable relative
  indexing. Strong on algorithmic tasks; mathematically specialized, little
  adoption.
- **Santoro, Bartunov, Botvinick, Wierstra & Lillicrap (2016). "Meta-Learning with
  Memory-Augmented Neural Networks."** ICML 2016 (PMLR 48:1842-1850).
  arXiv:1605.06065. NTM-style memory for one-shot learning, with a Least-Recently-
  Used-Access write rule; strong one-shot Omniglot. Showed external memory has
  value beyond toy algorithms; soon matched by simpler meta-learners (MAML,
  matching nets).
- **Shamshoum, Hodos, Sieradzki & Schuster (2024). "DNCs Require More Planning
  Steps."** arXiv:2406.02187. The DNC's internal "planning budget" (compute steps
  before output) determines learned time-complexity, stability, and length
  generalization on graph shortest-path / convex hull. Reframes some past failures
  as under-provisioned compute; confirms training remains slow/unstable.

**Sub-area arc.** From the 2014 NTM (a neural net *learning to use* a memory like
a programmable computer) to the 2016 *Nature* DNC peak, then two repair threads:
make memory tractable (sparse access) and make it stable (de-allocation fixes,
planning budget). The central insight — differentiable content-based attention
over an external store — worked best once *stripped of explicit read/write-head
machinery*: soft attention over memory (End-to-End Memory Networks → Transformers)
scaled on accelerators and dominated, while DNC-style models stayed pinned to
synthetic benchmarks. Interest waned over training instability, poor parallelism,
and scaling of dense addressing/linkage.

---

## 2. Memory-augmented networks beyond NTM/DNC, and attention as external memory

- **Weston, Chopra & Bordes (2014/2015). "Memory Networks."** arXiv:1410.3916.
  ICLR 2015. Inference component + a long-term, content-addressed memory array used
  as a dynamic knowledge base for QA. Original form needed strong per-step
  supervision.
- **Sukhbaatar, Szlam, Weston & Fergus (2015). "End-To-End Memory Networks."**
  arXiv:1503.08895. NeurIPS 2015. A fully differentiable Memory Network: soft
  attention over memory with multiple "hops", trained end-to-end. Direct
  conceptual ancestor of Transformer attention. Flat memory, limited capacity, no
  write/forget dynamics.
- **Miller, Fisch, Dodge, Karimi, Bordes & Weston (2016). "Key-Value Memory
  Networks for Directly Reading Documents."** arXiv:1606.03126. EMNLP 2016.
  Separate **key** (addressing) and **value** (returned content) encodings —
  prefigures the K/V structure of Transformer attention and modern memory layers.
- **Vinyals, Fortunato & Jaitly (2015). "Pointer Networks."** arXiv:1506.03134.
  NeurIPS 2015. Attention that *points at* input positions rather than blending
  them; established attention-as-addressing/selection (convex hull, TSP).
- **Grefenstette, Hermann, Suleyman & Blunsom (2015). "Learning to Transduce with
  Unbounded Memory."** arXiv:1506.02516. NeurIPS 2015. Differentiable Stack/Queue/
  DeQue controlled by an RNN; recovers underlying algorithms on synthetic
  transduction. Did not scale into mainstream use the way flat attention did.
- **Lample, Sablayrolles, Ranzato, Denoyer & Jégou (2019). "Large Memory Layers
  with Product Keys."** arXiv:1907.05242. NeurIPS 2019. A trainable ~1B-param
  key-value memory accessed sparsely via product keys at negligible FLOP cost; a
  12-layer memory model beat a 24-layer baseline at ~2x speed. Decouples capacity
  from compute.
- **Ramsauer, Schäfl, Lehner, et al. (2020). "Hopfield Networks is All You Need."**
  arXiv:2008.02217. ICLR 2021. A modern continuous Hopfield network storing
  exponentially many patterns, retrieving in one update — proven equivalent to
  Transformer attention. The theoretical bridge: self-attention *is* a
  content-addressable associative-memory read. (arXiv 2020; ICLR 2021 — secondary
  sources sometimes misstate "ICLR 2020".)
- **Lewis, Perez, Piktus, et al. (2020). "Retrieval-Augmented Generation for
  Knowledge-Intensive NLP Tasks."** arXiv:2005.11401. NeurIPS 2020. Parametric
  generator (BART) + a non-parametric, differentiable dense index of Wikipedia
  (DPR). Established "retrieve-then-generate" with an editable external corpus as
  memory. Retriever/index largely decoupled from generation.
- **Borgeaud, Mensch, Hoffmann, et al. (2022). "Improving Language Models by
  Retrieving from Trillions of Tokens" (RETRO).** arXiv:2112.04426. ICML 2022.
  Chunked cross-attention over a ~2T-token database via a frozen retriever; matched
  GPT-3 with ~25x fewer params. External memory as a substitute for parameters;
  bounded by frozen-retriever quality and train/retrieval leakage.
- **Wu, Rabe, Hutchins & Szegedy (2022). "Memorizing Transformers."**
  arXiv:2203.08913. ICLR 2022. Non-differentiable external memory of past (key,
  value) pairs queried by approximate kNN inside an attention layer; improved
  perplexity, scaled to 262K-token memory, reused newly defined functions at test
  time. Read-only KV caching (no learned write/forget); staleness as weights drift.
- **Bulatov, Kuratov & Burtsev (2022). "Recurrent Memory Transformer."**
  arXiv:2207.06881. NeurIPS 2022. "Memory tokens" carried across input segments
  give segment-level recurrence with no architectural change. Fixed-size token
  memory is a bottleneck; recurrence reintroduces sequential dependency.
- **Behrouz, Zhong & Mirrokni (2025). "Titans: Learning to Memorize at Test
  Time."** arXiv:2501.00663 (preprint; venue not confirmed). A deep long-term
  memory module that learns to write/forget at test time via a gradient-based
  "surprise" signal with momentum + adaptive forgetting, combined with attention.
  Revives explicit learned write/forget dynamics (NTM/DNC spirit) in a modern
  architecture.
- **Berges, et al. (Meta, 2024). "Memory Layers at Scale."** arXiv:2412.09764.
  ICLR 2025. Scales trainable product-key KV memory layers to 128B memory params
  (1T-token pretraining), beating dense models at 2x compute, largest gains on
  factual tasks. Direct scale-up successor to Product-Key Memory.

**Sub-area arc.** External memory began as explicit, structured modules (Memory
Networks, Key-Value, Pointer Nets, Neural Stacks). The decisive shift: soft
content-based attention IS an associative-memory read (End-to-End Memory Networks
→ Transformers; formalized by "Hopfield Networks is All You Need"). The field then
split into non-parametric retrieval (RAG, RETRO, Memorizing Transformers) and
parametric memory layers (Product-Key → Memory Layers at Scale), with a recent
return to learned recurrent write/forget (RMT, Titans). Open problems: capacity vs
interference, forgetting/staleness, *differentiable* addressing (most scalable
systems fall back to frozen/approximate kNN, breaking end-to-end learning), and
cheap consistent online *writes* without catastrophic interference.

---

## 3. Vector-symbolic architectures (VSA) / hyperdimensional computing (HDC) & neuro-symbolic

Symbolic operations as vector algebra: **bind** (associate), **bundle**
(superpose), **permute** (order), plus a **clean-up memory** to denoise.

- **Plate (1995). "Holographic Reduced Representations."** *IEEE TNN* 6(3):623-641.
  DOI:10.1109/72.377968. Circular convolution as binding (same fixed
  dimensionality), correlation as approximate unbinding; represents bindings,
  sequences, nested frames in fixed-width vectors. Binding is lossy; needs a
  clean-up memory.
- **Kanerva (1997). "Fully Distributed Representation" (Binary Spatter Code).**
  Proc. RWC'97, 358-365. Binary hypervectors, XOR binding, thresholded-majority
  bundling, Hamming similarity. The simplest, hardware-friendly VSA dialect.
- **Kanerva (2009). "Hyperdimensional Computing: An Introduction..."** *Cognitive
  Computation* 1(2):139-159. DOI:10.1007/s12559-009-9009-8. The canonical tutorial
  that named the field: quasi-orthogonality of random ~10,000-d vectors,
  concentration of measure, the bind/bundle/permute + clean-up algebra. An essay,
  not a new result.
- **Gayler (2003). "Vector Symbolic Architectures Answer Jackendoff's Challenges
  for Cognitive Neuroscience."** Proc. ICCS/ASCS. arXiv:cs/0412059. Coins "VSA";
  argues VSAs solve the binding problem, the problem of two, variable
  instantiation, working-vs-long-term memory. Presents MAP (Multiply-Add-Permute).
  Positional/conceptual.
- **Kleyko, Rachkovskij, Osipov & Rahimi (2022/2023). "A Survey on HDC aka VSA,
  Parts I & II."** *ACM Computing Surveys*. arXiv:2111.06077 (Part I, models/data
  transforms) and arXiv:2112.15424 (Part II, applications/cognitive models/
  challenges). The definitive two-part reference and map of the territory.
- **Kleyko, Davies, Frady, Kanerva, ... Sommer (2022). "Vector Symbolic
  Architectures as a Computing Framework for Emerging Hardware."** *Proc. IEEE*
  110(10):1538-1571. arXiv:2106.05268. Argues VSA is a natural programming model
  for stochastic, in-memory, neuromorphic substrates (local, parallel,
  noise-tolerant ops). The best bridge from VSA algebra to a "neural computer" in
  hardware. A roadmap.
- **Rahimi, Kanerva & Rabaey (2016). "A Robust and Energy-Efficient Classifier
  Using Brain-Inspired Hyperdimensional Computing."** ISLPED'16.
  DOI:10.1145/2934583.2934624. HDC as a practical low-power *learning* method:
  encode → bundle class prototypes → similarity search; fault-tolerant. Shallow
  single-pass learning trails deep nets on hard perception.
- **Frady, Kent, Olshausen & Sommer (2020). "Resonator Networks, 1 & 2."** *Neural
  Computation* 32(12):2311-2331 and 2332-2388. DOI:10.1162/neco_a_01331. Solves
  VSA's inverse problem: factor a bound product into its unknown factor
  hypervectors via recurrent superposition + clean-up, far more efficiently than
  gradient descent. Finite operational capacity. (Page ranges from search
  metadata; publisher page returned 403.)
- **Mao, Gan, Kohli, Tenenbaum & Wu (2019). "The Neuro-Symbolic Concept Learner."**
  ICLR 2019. arXiv:1904.12584. Neural perception front-end + symbolic program
  executor, learned jointly from image-QA pairs with no intermediate supervision;
  interpretable, data-efficient compositional reasoning (CLEVR). Relies on a fixed
  DSL + curriculum.
- **Manhaeve, Dumančić, Kimmig, Demeester & De Raedt (2018). "DeepProbLog: Neural
  Probabilistic Logic Programming."** NeurIPS 2018; extended in *Artificial
  Intelligence* (2021), arXiv:1907.08194. ProbLog + neural predicates whose
  probabilities come from neural nets; end-to-end perception + probabilistic-
  logical inference. Inference cost grows with the program; hard to scale.
- **Riegel, et al. (2020). "Logical Neural Networks."** arXiv:2006.13155. Each
  neuron is a connective/literal in a weighted real-valued logic; the net is a
  directly interpretable, omnidirectional-inference logical formula with
  contradiction tolerance. Needs substantial logical structure specified; limited
  large-scale validation.

**Sub-area arc.** VSA/HDC supplies a neural computer its *datatype and instruction
set*: fixed-width hypervectors plus bind/bundle/permute and clean-up. Quasi-
orthogonality + noise-robustness yield real compositionality, transparent
(interpretable) operations, and computing-in-superposition. What works today:
energy-efficient fault-tolerant classification (great fit for neuromorphic/
in-memory hardware), robust encoding/querying of structured records, and
factorization via resonator networks; neuro-symbolic hybrids add data-efficient,
auditable reasoning over learned perception. Open problems: capacity/crosstalk
limits, shallow learning vs deep nets, scaling logic inference, and hardware
co-design.

---

## 4. Differentiable computation, program induction, and the LLM-OS framing

- **Zaremba & Sutskever (2014). "Learning to Execute."** arXiv:1410.4615. LSTM
  seq2seq reads short program source and predicts output; RNNs can approximate
  execution (single-pass, constant-memory) with curriculum learning. A statistical
  approximation, not a verifiable interpreter; degrades on nesting.
- **Neelakantan, Le & Sutskever (2015). "Neural Programmer: Inducing Latent
  Programs with Gradient Descent."** arXiv:1511.04834. ICLR 2016. A small fixed set
  of discrete arithmetic/logic ops, learned to be selected+sequenced over tables
  with weak (I/O) supervision. Tiny hand-designed op set; short programs.
- **Reed & de Freitas (2015). "Neural Programmer-Interpreters (NPI)."**
  arXiv:1511.06279. ICLR 2016. Compositional: task-agnostic core + learnable
  key-value program memory + domain encoders; calling lower-level programs from
  higher gives strong generalization and low sample complexity. Needs full
  *execution traces* (expensive supervision).
- **Kaiser & Sutskever (2015). "Neural GPUs Learn Algorithms."** arXiv:1511.08228.
  ICLR 2016. Computationally-universal, highly parallel (convolutional GRUs);
  trained on ~20-bit binary add/multiply, generalized error-free to far longer
  inputs. Later shown fragile/sensitive to training regime
  (arXiv:1611.00736).
- **Graves et al. (2016). Differentiable Neural Computer.** (See §1; the flagship
  "differentiable computer".)
- **Gaunt, Brockschmidt, Singh, Kushman, Kohli, Taylor & Tarlow (2016). "TerpreT:
  A Probabilistic Programming Language for Program Induction."** arXiv:1608.04428.
  A DSL + interpreter with four back-ends (gradient descent / LP / SAT / Sketch),
  enabling a controlled comparison. Sobering finding: constraint/SAT solvers
  consistently beat the gradient-descent and LP formulations — a strong negative
  result for "just make the interpreter differentiable".
- **Bošnjak, Rocktäschel, Naradowsky & Riedel (2017). "Programming with a
  Differentiable Forth Interpreter (∂4)."** arXiv:1605.06640. ICML 2017. A
  differentiable Forth: write a program *sketch* with trainable slots filled from
  I/O data; injects procedural priors, helps in low-data regimes. Small-scale;
  differentiating a full stack machine is expensive.
- **Dehghani, Gouws, Vinyals, Uszkoreit & Kaiser (2018). "Universal Transformers."**
  arXiv:1807.03819. ICLR 2019. Recurrence-in-depth + per-position adaptive halting;
  Turing-complete under idealized assumptions (unlike fixed-depth Transformers).
  Bridges the explicit neural-computer line to modern attention.
- **Packer, Wooders, Lin, Fang, Patil, Stoica & Gonzalez (2023). "MemGPT: Towards
  LLMs as Operating Systems."** arXiv:2310.08560. Borrows OS virtual-memory paging:
  a fixed-context LLM pages data between in-context "main memory" and external
  "disk". **An orchestration/memory-management layer around a frozen model**, not
  differentiable computation.
- **Mei, Li, Xu, Wang, Rao & Zhang (2024). "AIOS: LLM Agent Operating System."**
  arXiv:2403.16971. COLM 2025. An "AIOS kernel" factoring agent scheduling, context/
  memory, storage, tool access out of agent apps; ~2.1x faster serving. Systems
  engineering for agents, not learnable computation.
- **Karpathy (2023). "LLM OS."** Public talk + X thread (NOT peer-reviewed —
  flagged). The influential metaphor of the LLM as CPU/kernel orchestrating tools,
  code, browsing, and a memory hierarchy. Conceptual; no results.

**Sub-area arc.** The classic line (2014-2018) tried to make computation itself
learnable — differentiable memory (NTM/DNC), universal parallel nets (Neural GPU),
compositional program callers (NPI), latent-program inducers, differentiable
interpreters (TerpreT, ∂4). Proof-of-concept worked (gradient descent can induce
copy/sort/add; priors + compositionality help sample efficiency), but scaling and
robustness failed — soft addressing brittle, training unstable, generalization
fragile, NPI needs costly traces, and TerpreT's verdict was blunt: discrete SAT
solvers beat differentiable relaxations at real synthesis. The recent "LLM-OS"
framing (MemGPT, AIOS, Karpathy) is categorically different: the model is frozen/
non-differentiable and the "OS" is orchestration scaffolding. Through-line: the
dream of "computer as learnable object" — but the two eras answer it oppositely
(learn the computer vs. treat a trained model as the computer and wrap an OS
around it).

---

## 5. Interpretability and the "readable computation" angle

- **Olah, Cammarata, Schubert, Goh, Petrov & Carter (2020). "Zoom In: An
  Introduction to Circuits."** *Distill*. The founding "circuits" statement:
  networks reverse-engineer into meaningful subgraphs of interpretable features +
  weighted connections; features-as-units, circuits, universality. Developed on
  vision CNNs; manual and labor-intensive.
- **Elhage, Nanda, Olsson, et al. (2021). "A Mathematical Framework for Transformer
  Circuits."** *Transformer Circuits Thread* (Anthropic). Residual stream as
  channel; QK/OV circuits; head composition; identifies induction heads in toy
  models. Clean only for 0-2 layer attention-only models.
- **Olsson, Elhage, et al. (2022). "In-context Learning and Induction Heads."**
  *Transformer Circuits Thread*. Induction heads (match-and-copy) as a primary
  mechanism of in-context learning, tied to a training phase change. Strongest
  causally in small models.
- **Elhage, Hume, Olsson, Schiefer, Henighan, et al. (2022). "Toy Models of
  Superposition."** *Transformer Circuits Thread*. Networks store more features
  than dimensions by overlapping them → polysemantic neurons; explains why reading
  neurons fails and motivates dictionary learning. Toy settings.
- **Bricken, Templeton, Batson, ... Olah, et al. (2023). "Towards Monosemanticity:
  Decomposing Language Models with Dictionary Learning."** *Transformer Circuits
  Thread*. Sparse autoencoders recover many more monosemantic, steerable features
  than neurons. Demonstrated on a one-layer model.
- **Templeton, Conerly, ... Henighan, Olah, et al. (2024). "Scaling
  Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet."**
  *Transformer Circuits Thread*. Scales SAEs (~34M features) to a production model,
  with causally steerable safety-relevant features ("Golden Gate Claude").
  Dictionary admittedly incomplete.
- **Rudin (2019). "Stop Explaining Black Box Machine Learning Models for High
  Stakes Decisions and Use Interpretable Models Instead."** *Nature Machine
  Intelligence* 1:206-215. DOI:10.1038/s42256-019-0048-x. Post-hoc explanations of
  black boxes are by definition not faithful; build inherently interpretable models
  instead, often at little accuracy cost. Framed largely for tabular data.
- **SAE-faithfulness critique cluster (2024-2025):** "Towards Principled
  Evaluations of Sparse Autoencoders" (arXiv:2405.08366); "Automatically
  Interpreting Millions of Features in LLMs" (arXiv:2410.13928); "Evaluating SAE
  interpretability without explanations" (arXiv:2507.08473). Document feature
  splitting, feature absorption, large linearly-predictable reconstruction error,
  and SAEs sometimes losing to probing/steering baselines. (arXiv ids verified by
  search; per-paper attributions approximate — double-check before precise
  citation.)
- **Lindsey, Gurnee, Ameisen, ... Batson, Olah, et al. (2025). "Circuit Tracing:
  Revealing Computational Graphs in Language Models" / "On the Biology of a Large
  Language Model."** *Transformer Circuits Thread*. Cross-layer transcoders +
  attribution graphs trace multi-step computation (planning, multilingual circuits,
  arithmetic, hallucination) in Claude 3.5 Haiku, validated by interventions.
  Graphs are local (per-prompt), partial, depend on a replacement model.
- **Yu, Buchanan, Pai, Chu, Wu, Tong, Haeffele & Ma (2023). "White-Box
  Transformers via Sparse Rate Reduction" (CRATE).** NeurIPS 2023.
  arXiv:2306.01129. Each layer is one step of an optimization (attention compresses
  toward incoherent subspaces; MLP sparsifies), so the architecture *is* a known
  algorithm — interpretable by construction, competitive with ViT, with emergent
  segmentation. Accuracy trails engineered transformers; mostly vision.
- **Koh, Nguyen, Tang, Mussmann, Pierson, Kim & Liang (2020). "Concept Bottleneck
  Models."** ICML 2020. arXiv:2007.04612 (id recalled — flag). Predict
  human-specified concepts first, then the label only from those concepts;
  reasoning runs through named, intervenable variables. Needs concept annotations;
  concept leakage.

**Sub-area arc.** Two strategies. **Post-hoc recovery** — circuits, SAE feature
dictionaries, per-prompt attribution graphs — has produced readable artifacts at
frontier scale, but rests on superposition making neurons unreadable, and its main
tool (SAEs) is now shown to fragment/absorb features, leave large unexplained
error, and sometimes lose to trivial baselines, so the recovered "program" is
incomplete and possibly partly artifactual. **Interpretable-by-construction** —
Rudin's argument, concept bottlenecks, white-box transformers (CRATE) — pays an
accuracy/scale tax and constrains representations. The open problem is faithfulness
and completeness. This is exactly why an **inspectable substrate** (where the
program *is* its own execution trace, read rather than reconstructed) would sidestep
the faithfulness gap: there is no recovered approximation to be wrong.
