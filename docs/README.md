# Baterm Docs — Navigation Index

> **This page is the first thing any AI agent or new contributor should read.**
> Skim it, then dive into the specific document you need.

---

## What is this folder?

`docs/` is the **single source of truth** for Baterm's project knowledge. Every architectural decision, every product requirement, every code-style rule lives here. The code is downstream of these documents; the documents are not downstream of the code.

When you (human or AI) make a decision, **update the relevant document first**, then write code. When you discover something contradicts what a doc says, **fix the doc, don't work around it**.

---

## Folder structure

```
docs/
├── README.md                  ← you are here
├── project/                   ← what the product IS
│   ├── vision.md              product vision, core values, non-goals
│   ├── architecture.md        top-level architecture, module boundaries, key constraints
│   └── glossary.md            unified Chinese/English terminology
├── decisions/                 ← WHY we made each significant choice
│   └── YYYY-MM-DD-title.md   one file per significant decision
├── guidelines/                ← HOW we write code and work
│   ├── style-guide.md         code style, naming conventions
│   ├── commit-convention.md   commit message format
│   └── agent-prompt.md        behavioral rules for AI agents (CRITICAL)
├── product/                   ← WHAT we're building next
│   ├── roadmap.md             high-level version planning
│   └── stories/               user stories + acceptance criteria
│       └── epic-XX-story.md  organized by epic or module
└── process/                   ← HOW we collaborate and learn
    ├── changelog.md           user-facing release notes
    ├── retrospectives/        post-iteration retrospectives
    │   └── sprint-XX.md       AI agents: read these to learn past mistakes
    └── templates/             reusable document templates
        ├── decision-template.md
        └── story-template.md
```

---

## Reading order for a new AI agent

If you have never seen this project, read in this order:

1. **`project/vision.md`** — what is Baterm, who is it for, what does it explicitly NOT do
2. **`project/architecture.md`** — what are the modules, what are the boundaries, what are the constraints
3. **`project/glossary.md`** — the exact Chinese/English term mapping used throughout
4. **`guidelines/agent-prompt.md`** — the behavioral rules you must follow when acting on this codebase
5. **`decisions/`** — read the most recent 5–10 decision files; they explain the WHY behind the current shape
6. **`process/retrospectives/`** — read the most recent 2–3 retrospectives; they are the closest thing to "lessons learned"

If you only have 5 minutes, read vision.md + agent-prompt.md. That combination is enough to avoid the most common mistakes.

---

## Reading order for a human contributor

1. **`project/vision.md`** — does this project match what you expected?
2. **`project/architecture.md`** — what would you touch, what should you leave alone?
3. **`decisions/`** — pick the decisions relevant to the area you're working in
4. **`guidelines/style-guide.md`** + **`guidelines/commit-convention.md`** — before writing your first PR
5. **`product/roadmap.md`** — what's coming up, where can you plug in

---

## Document conventions

- **One fact, one source.** If a fact appears in two places and they conflict, the more specific doc wins. (vision beats README beats CHANGELOG for product direction.)
- **English-first for technical content.** Code, identifiers, commands, and config keys are English. Comments may be English or bilingual Chinese/English. Long-form narrative (vision, retrospectives) may be primarily Chinese when the author prefers; this is a deliberate bilingual tolerance.
- **Dates in `YYYY-MM-DD`** (ISO 8601). Avoid `07/05/26` or `2026/7/5`.
- **No placeholder text.** Never ship `xxx`, `TBD`, `TODO: fill this in`, or `...` in committed docs. If a section is incomplete, write that explicitly: `Status: incomplete, blocked on <reason>`.
- **Revision markers.** When a doc is updated, add a one-line entry at the bottom of the affected section: `> v0.2 (2026-07-05): added Free-tile design constraint`. Full revision history lives at the top of each doc.

---

## Editing rules

| You want to ... | Do this |
|-----------------|---------|
| Add a new user story | Create `product/stories/epic-XX-name.md` using `process/templates/story-template.md` |
| Record a significant decision | Create `decisions/YYYY-MM-DD-short-title.md` using `process/templates/decision-template.md` |
| Reflect on a past iteration | Create `process/retrospectives/sprint-XX.md` |
| Update product direction | Edit `project/vision.md` and add a revision marker |
| Change a code-style rule | Edit `guidelines/style-guide.md` and add a revision marker |
| Update roadmap | Edit `product/roadmap.md` and add a revision marker |
| Add a release note | Edit `process/changelog.md` (newest entry at top) |

Every change **must** leave a paper trail. If you can't point at the doc that explains a choice, the choice isn't real.

---

## What this folder is NOT

- It is not a marketing surface — that's the top-level `README.md`.
- It is not a tutorial — that's `docs/guidelines/` and external guides.
- It is not an issue tracker — use GitHub Issues for that.
- It is not a wiki where anyone can rewrite history — every doc has an author and revision markers.

---

## AI agent guardrails (read this before doing anything)

1. **Read before writing.** If a doc exists on the topic, you must read it. If you disagree, update the doc; do not silently contradict it.
2. **Cite when you change.** When your code change comes from a decision doc, mention the decision file in your commit message.
3. **No speculative docs.** Do not create empty or placeholder docs to "make the folder complete". Empty folders are fine; empty docs are not.
4. **Respect scope.** If you are asked to update `product/vision.md`, do not silently also update `architecture.md`. Either ask, or do them as separate commits with separate explanations.

See `guidelines/agent-prompt.md` for the full behavioral contract.

---

> Last updated: 2026-07-05 (initial baseline; v0.5.2 design freeze)