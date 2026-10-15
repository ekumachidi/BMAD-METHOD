---
tracker_id: ""   # with remote (url): set at publish on a tracker, on a story by `tickets.py mirror`; cut on the repo store
key: ""   # container: tracker project or team for its subtree, if not the store's
id: [story: its tickets.toml entry id, or the folder's `next_id` if none; cut on a container]
type: [initiative | epic | story]
title: "[Container: its outcome. Story: what works when done]"
parent: [the container folder above; none for an initiative or a backlog/ story]
covers: [epic: parent ids it owns; story: epic ids it serves; cut on an initiative. Constraints go in References, not here]
sources: []   # every input SPEC derived Requirements from, paths from {project-root}; each is re-read on update; never the memlog
companions: []   # files downstream reads beside this ticket, written or adopted by SPEC, paths from {project-root}
after: []   # container: epics it waits on; epic order is in tickets.toml. Story: prerequisites only: sibling id, "<epic id>.<entry id>", or epic-<slug>
assignee: ""   # container: gains status (in-progress | done | dropped) at work start. Story: only a tracker's, via `tickets.py mirror`
# a story's status, assignee, blocked_at, blocked_reason live in the plan beside this file, not here; tracker_status from tracker sync
refined: false   # story only, cut on a container: true once the user approves its criteria; on a pulled story only if its entry has `refine = true`
hitl: false   # story only, cut on a container
risk: [low|medium|high; container: a best guess until sliced, then its children's highest]
---

<!-- At slicing a container gets all but Requirements and Constraints, which SPEC writes at the split or from an input. A pulled or reviewed story: Description, Verify: line, References, local Notes; refining adds numbered criteria and Boundaries. Done and dropped tickets stay as written. An initiative is the business outcome its epics serve, usually spanning quarters and more than one boundary; for a solo developer or small team a whole product is a fine initiative. -->

# [Title]

## Description

[Container: a paragraph, what is true when done and why, pointing at the inputs or parent. Story: one sentence; with numbered criteria, 2–4 on how it advances the epic, not restating them.]

## Outcome

[Container: one sentence: for whom, what changes, the source's success signal named not restated. Story: cut.]

## Requirements

[SPEC writes this from the ticket's `.memlog.md`; Sofia edits it, never slicing. Initiative: `R<n> (<source ids>): <checkable result>`, ids prefixed with the source's References type when ambiguous (`prd FR-2`), its section when it has none. Epic: its owned parent lines, `R<n> (parent R<m>): <the line>`, deepened only as inputs require; split or added lines name a parent id or source section, all lines a source section when `covers` is empty. Story: only `Verify: how it will be checked`. No epic, `refine = true`, or on request: numbered criteria, each naming the epic ids it proves; a criterion states a rule, not an instance, with its failure path.]

1. **[Behavior name]** (R<m>)
   **Given** [state before]
   **When** [the one action]
   **Then** [what is observable]
   **And** [another observation, if any]

## Constraints

[SPEC writes this. Initiative: each design-bending non-negotiable, citing its source id or section. Epic: the parent's that bind it. Story: only ones beyond its epic's; behavior to keep goes under Boundaries.]

## Done when

[Container: three to six end checks from the source's measures and limits: an initiative's business outcomes, an epic's deliverables at its Requirements' altitude, production delivery among them. Closing every child is not one. Story: cut.]

## Boundaries

[Container: its boundary and what it is not, from the source's non-goals. Initiative: plus a line per touched unit with no epic, and a one-sentence tracer path across epics. Epic: what belongs to siblings. Story: with numbered criteria, adjacent behavior an implementation could break, not files; else cut.]

- Touch point: [unit] — [what is used there]; owner: epic-[slug]
- Must not change: [behavior]

## References

[`type — location, section`, location a path from {project-root} or a url. A line per companion SPEC writes or adopts, typed by content (glossary, design, architecture, prd). Initiative: section pointers into its sources and companions. Epic: the parent's Requirements, then the architecture spine section and design it adopts. Story: the parent, plus what the epic's lack.]

- parent — [path or url, section Requirements]
- [companion — a file beside this one, named for its content]
- [design — an adopted companion (../ux-<slug>/DESIGN.md) or a url]

## Notes

[Only what is unsettled here and not in the repo or source, one marked line each; cut if empty. SPEC writes Assumption: and Open question: from the memlog; slicing and refining, the rest.]

- Assumption: [an unconfirmed choice; once confirmed, a dated Decision]
- Open question: [container: what the source leaves open, and who waits; story: only its own, answered in its work]
- Unknown: [the gap and the entries waiting on it]
- Parked: [an unplaced requirement id, why, with the user's knowledge]
- Decision: [the user's choice, dated; a declined suggestion too]
- Source conflict: [id or section — source vs code or another source]
- Waits on [epic] because: [reason, one per after entry]
- Dropped: [why, before status goes to dropped]
