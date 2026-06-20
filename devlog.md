# neural-computers — Devlog

**This file is where "done" lives.** `queue.md` is delete-only: when a queue
item is finished, the item is **deleted from `queue.md`** and a dated entry
is **appended here**, in the same commit as the work, then pushed. Never
tick a box in place — a checked box left in `queue.md` is the failure mode
this file exists to prevent.

Also record releases (tag + a one-line note), notable milestones, and
anything else worth a chronological trail. Newest entries at the bottom.

This is the **same convention as the cleanvibe repo's own `devlog.md`** —
every cleanvibe-scaffolded project gets one for the same reason.

See `CLAUDE.md` § "Workflow Rules" and `queue.md`'s preamble.

---

## 2026-06-20 — Project scaffolded

Scaffolded with `cleanvibe research` (cleanvibe v1.13.1). Future entries
land here as queue items get deleted.

## 2026-06-20 — Bootstrap: question + literature review (agentic RAG)

- **Research question** set at scaffold time via `--question` (neural computers:
  the differentiable/memory-augmented line, neural approaches to general-purpose
  computation, and where interpretable / neuro-symbolic / VSA approaches fit).
  Reflected in README.md, CLAUDE.md, and `docs/index.html`.
- **Triage (`data_lake/`):** nothing user-supplied; nothing to move.
- **Literature review (agentic RAG):** five parallel web-research agents, one per
  sub-area — (1) NTM/DNC lineage, (2) memory-augmented nets + attention-as-memory,
  (3) VSA/HDC + neuro-symbolic, (4) differentiable computation / program induction
  / LLM-OS, (5) interpretability. ~55 sources gathered, citations verified against
  the originals (unverifiable details flagged). Wrote `literature/sources.md` (the
  cataloged source notes) and `literature/REVIEW.md` (synthesized survey: the
  attention/associative-memory convergence, the VSA alternative, the negative
  results, the post-hoc-vs-by-construction interpretability split, the gap, and
  the inspectable-substrate contribution this project tests).
- **`todo.md`** written: the inspectable-substrate thesis + four experiments
  (E1 expressivity-vs-legibility, E2 capacity-in-superposition, E3 faithfulness
  by-construction-vs-recovered, E4 the honest comparison) + report shape.
- **`docs/index.html`** report updated (lede, "grounded in the literature" pillar,
  finding pillar, findings status). `queue.md` reduced to the post-bootstrap state
  (E1 is the next real item).
- NOTE: done synchronously in one interactive session — the three-cron autonomous
  playbook was intentionally NOT started (the user asked to barrel through now).
  Experiments (E1-E4) are not yet run; they are the next session's work.
