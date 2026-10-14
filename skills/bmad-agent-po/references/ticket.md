# Writing a ticket

Ask only what is unsettled, with a default for each. Under an epic with a breakdown, a ticket is an entry with the keys `{{ workflow.tickets_template }}` shows, in build order, with a file only once pulled; a standalone ticket is a `backlog/` file. Take its `id` from `next_id` in `uv run {skill-root}/scripts/tickets.py --project-root {project-root} status <folder>`. Open only the template for its type: `{{ workflow.ticket_template }}` (initiative, epic, story), `{{ workflow.spike_template }}` or `{{ workflow.bug_template }}`. Match its example's detail, not its content.

{{ workflow.refine_before }}

## Each fact lives in one place

Reference each level's requirement source; never copy it. The container owns Description, Outcome, Done when, Boundaries, References, Notes. An epic's `covers` lists the parent ids it owns and each Requirements line names one; an initiative has no `covers`, and each line names its source ids. SPEC writes Requirements, Constraints, the `Assumption:` and `Open question:` lines, and a References line per companion; refining writes a story's criteria in Requirements; slicing writes the rest.

A story's `covers` cites ids from its epic's Requirements. Full criteria ("Reviewing or refining an existing ticket" says when) cover what changes, failure paths and binding decisions; point at the source for the rest.

## Rules the template cannot carry

- Criteria use the template's format, each provable without pointing at code, together failing any wrong build. Behavior that must not change goes under Boundaries.
- Short plain sentences in the project's vocabulary, no invented terms. Offer the user a rewrite where their text breaks this.
- A reference is a References line, `type — location, section`; location is a path from `{project-root}` or a url, opened before citing; cite the nearest document. Before drafting, read `tickets.reference` from the store config (`read_store.py -k tickets`; the repo store has none).
- No source-code paths or snippets unless the snippet is the decision; a path the user wants kept goes in Notes.
- A UI ticket links its design in References; criteria stay functional. User-facing without a design: offer `bmad-ux`; if declined, warn that the builder will guess the layout unless Notes gives details.
- `hitl: true` only when a person does part of the work; name that step in the Description and list known steps in criteria or Notes.
- Risk on every ticket: `low`, `medium` or `high` business impact if wrong, proposed with a one-line reason; the user decides. A high-risk ticket names in Notes one check beyond its criteria.
- Record an `estimate` only when the user gives one, on the entry or frontmatter; never propose one.
- A bug carries reproduction steps and a cause hypothesis, never a fix; ask for missing steps, tighten unclear ones. Run them when cheap; if the behavior already holds, show the evidence and create nothing. Criteria name the other valid outcome: proof no change is needed.
- A spike names the question, who waits on it, and where the answer goes. A question several epics wait on is a shared decision per `slice.md`.
- Notes holds marked lines local to this ticket; what spans tickets goes in the parent's Notes, referenced. Confirm each `Assumption:` with the user; the answer is a dated decision. An `Open question:` is answered during the work. An `Unknown:` gates the start (`tickets.py next` lists it under `blocked`): settle it with the user as a dated decision, or as a spike in its dependents' `after` when they ask. Never guess. With a `.memlog.md`, `Assumption:` and `Open question:` lines go to the memlog first (`spec.md`, Memory and derivation).

## Reviewing or refining an existing ticket

Resolve the user's ref with `uv run {skill-root}/scripts/tickets.py --project-root {project-root} find [<folder>] <ref>`. If `story_file` is null, run `tickets.py pull <epic folder> <id>` (same `--project-root`); it writes a fixed layout, not the template. Leave done or dropped tickets and plans as they are.

Full criteria are written only for a bug, a ticket with no epic, and an entry with `refine = true`, set only when the user asks; `tickets.py next` lists these under `ready_to_refine`. Other stories go to the build as they are; a request to refine one is a review unless the user asks for full criteria.

`title`, `type`, `after` and `hitl` change in the file and the entry (the file's `after` and `hitl` win on `drift`); full criteria asked for on a story set `refine = true` on the entry and `refined: false` in the file; order lives in `tickets.toml`. Once published, changes also go through `write`.

**Review** — the next story, or more on request. Read the file, its source, and finished siblings and their plans. With the user, improve the ticket, order and prerequisites; run the Set check in `validate.md` when order or prerequisites change. Set neither `status` nor `refined`.

**Refine** — before the ticket starts. Read what a review reads, plus the code it touches. Where the source contradicts the code or another source, add `Source conflict: <id or section> — <what the source says> vs <what was found>` to the owning container's Notes, tell the user, and offer to log the correction through `spec.md`; tickets covering that id stay `refined: false` until a `Decision:` line settles it. Confirm prerequisites are done or in review and expand per the type's template and "Rules the template cannot carry".

**A ticket that already carries full criteria, or a container** — with the user, confirm they still agree, check it against its references, tracker state and current requirement source, and settle questions blocking implementation. A container may end in a re-slice per `slice.md`.

A reply approves only what was presented; publishing and starting work are separate approvals unless asked for together. Preserve identity, `status`, `tracker_status` and assignee.

## Self-review before the user sees it

Check its level (an entry: description and verification approach; refined: runnable criteria, settled prerequisites), size (one agent session), references and source consistency. Fix, then mention fixes that change intent.

## After approval

On the repo store, save through `write`; on a tracker it stays in the tree until published per `board.md`. A ticket with full criteria gets `refined: true` on the user's approval, then the Ticket check in `validate.md` before a build starts it.

{{ workflow.refine_after }}
