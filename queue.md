# neural-computers — Work Queue (research)

**A queue of concrete, executable steps, not a state snapshot.** When an item is
done, delete it from here AND append a dated entry to `devlog.md` in the same
commit, then push. No checkmarks or "done" markers in place. Longer-horizon,
abstract work lives in `todo.md` and gets decomposed into items here when ready.

Bootstrap is complete: research question set; **literature review done**
(`literature/sources.md` + `literature/REVIEW.md`, agentic-RAG, cited);
`todo.md` written; report (`docs/`) updated; repo live on GitHub Pages. See
`devlog.md` for the bootstrap entries.

---

## Active — real research (decompose from todo.md when starting)

1. **Decompose E1 (expressivity vs. legibility) into concrete steps and run it.**
   Pull E1 from `todo.md`: pick the small ladder of algorithmic tasks (copy /
   associative recall / graph traversal), define the legibility metric, implement
   under `src/` with `scripts/run.py` as the entry point, write metrics to
   `results/`, and keep `FINDINGS.md` + the `docs/` report current. Then pull
   E2-E4 as the queue drains. (Not started — this is the next session's work.)

---

## Always last — restart crons + summarize

A. **(Autonomous sessions only) ensure the three crons are running** — work-loop
   `3 * * * *`, auto-flush `15 * * * *`, status-report `42 * * * *`. NOTE: the
   bootstrap pass was done synchronously in one interactive session, so no crons
   were started; start them if a future session runs the experiments
   autonomously.
B. **Run the status-report action once more, independently** — end-of-session
   summary.
