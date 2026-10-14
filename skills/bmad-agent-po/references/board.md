# Board

Read a ticket, and its remote state if published, before changing it.

`tickets.py` below is `uv run {skill-root}/scripts/tickets.py --project-root {project-root}`; `--help` on a command gives its arguments and output keys.

{{ workflow.board_before }}

## Reading the tree

- Resolve a named ticket with `tickets.py find [<folder>] <ref>`; `<ref>` is `<epic id>.<entry id>`, an entry id with its epic folder, a tracker id, a file name, or an unbroken title phrase. For looser words, pick from `status`, asking when several fit. `find`'s `story_file` is null until pulled; `plan` is the plan's path, existing or not.
- `tickets.py next [<folder>]` groups open tickets by next step; `tickets.py status [<folder>]` lists every ticket.
- `<folder>` is an epic folder name, `backlog`, or the initiative, by default the active one, where `next` and `status` add a `backlog` key to show.
- With a tracker, sync first (Tracker sync) and pass `--synced` to `next`.

Act with the user on any non-empty key:

- `unpinned_after`: add the epic's `after` prerequisite to the entry that needs it.
- `undeclared_after`: add the awaited epic, with its `needs`, to its epic's `after` in the breakdown, or drop the prerequisite.
- `order_conflict`: reorder or merge the epics, or record why in a `Decision:` line.
- `drift`: make the leaf file's and the entry's `after` or `hitl` equal.
- `problems`: fix the plan; `mark` sets a valid status.
- `waiting_on` on a `blocked` row: say what the ticket waits on.
- `unknown` on a `blocked` row: settle it, remove it, and add a dated `Decision:` line to the file's Notes, or its epic's when it has no file.

## Publish

Approval records a breakdown in the epic's `tickets.toml` and starts no work. On the repo store, committing it is the publish. On a tracker, publication is `{{ workflow.publication }}`: `at_inception` offers to publish the agreed set after validation; `on_start` publishes each ticket when it starts. An explicit user request overrides it.

Publish to a tracker through `write`:

- Write the leaf's file first (`tickets.py pull` if none); send it minus the frontmatter lines `tracker_id`, `remote`, `tracker_status`, `status`, and `assignee`.
- Create parents and prerequisites first, adding local prerequisites to the scope. An `after` naming a sibling's id or `<epic id>.<entry id>` is that entry's remote item; `epic-<slug>` is the epic's.
- A type mapped to `""` is never sent; its children attach to the nearest published ancestor. Say so once.
- Offer `setup` at first publish, or when the tracker lacks something the maps or `fields` name.
- Then mirror, and edit a published container's `tracker_id` and `remote` into its file.

An unrefined ticket may be published with `refined: false`; refinement updates the same file and remote item.

## Start

Offer the first `ready_to_start` row of `next` in build order, or every unassigned one when work can run in parallel. Read it and its source (`find` with the row's `ref`), refining a `ready_to_refine` one first per `ticket.md`, and confirm it is unassigned. With the user's approval, publish it if needed and start it: on a tracker, `write` it to `[tickets.status].in-progress` and mirror; then hand it to `bmad-build` or `bmad-build-auto`.

Only an unattended run's orchestrator honors the row's `plan_checkpoint` and `done_checkpoint`.

## Status

A leaf's `status`, `blocked_at`, and `blocked_reason` live in its plan, `<type>-<slug>-plan.md` beside where its file is or would be, linked by the plan's `ticket`; the leaf file's `status` counts only without a plan.

`bmad-build` and `bmad-build-auto` write `draft`, `ready-for-dev`, `in-progress`, `in-review`, `built`, and `blocked`; the user or an orchestrator sets `done` through `tickets.py mark`; Sofia sets `dropped` when the user says so.

Sofia changes a leaf's status or assignee only when the user asks (`done`, `dropped`, or a person working it by hand); when the user says a ticket is done, mark it.

- **Repo store.** `tickets.py mark [<folder>] <ref> <status> [--assignee <who>] [--blocked <reason>]`, then the commit `write` describes. `mark` writes only the plan; the checks here are yours.
- **Tracker.** `write` transitions the item to `[tickets.status].<state>`, with `<state>` as `tickets.py status` reports it, never a `status` word, and sets its assignee; then mirror. `mark` refuses a tracker store.
- **Waiting on a person or an answer** on the repo store: `mark` with the current status (`draft` when none) and `--blocked <reason>`; a `mark` without it clears the block.
- **A container's** `status` is Sofia's to edit in its file: absent until its first ticket starts, then `in-progress`, then `done` or `dropped` on the user's confirmation; a published one transitions through `write`.

## Close, drop, move

- {{ workflow.board_closure }}
- Drop only after a `Dropped:` line in the ticket's Notes, or its epic's when it has no file, says why; then set `dropped` per Status; an unpublished entry takes `tracker_status: dropped` through `mirror`. Never delete an entry from an approved `tickets.toml`. With the user, remove or repoint the dropped ticket in each dependent's `after` (`status` lists them under `blocks`). Cancelling a container cancels its descendants once the user confirms.
- To move a leaf: write the old folder's `next_id` (from `tickets.py status <folder>`) atop its `tickets.toml` as `next_id = <n>`; move the entry to its build-order place in the new `tickets.toml`, with the new folder's `next_id` as its id; move the leaf file and plan; set the file's `id` and `parent` and the plan's `ticket` to the new values. Rewrite its own `after` (a former sibling becomes `<epic id>.<entry id>`) and every `after` naming it. Move its `covers` ids between the two epics if the requirement moves. An epic moves as its whole folder with its `[[epic]]` table. Then run `tickets.py status` on the initiative and clear what it reports.
- A done standalone ticket stays in `backlog/` beside its plan.

## Tracker sync

After every `write`, and after the `query` before a board view, run `tickets.py mirror [<folder>]` with a JSON array on stdin, one object per ticket: `ref` (or its `tracker_id`) plus any of `tracker_id`, `remote`, `tracker_status`, `assignee`, `after`.

- `tracker_status` is the state mapped back through `[tickets.status]`: after `write`, the state sent; when two states share a tracker status the query cannot tell apart, the current one if it matches, else the earlier.
- `after` holds tracker ids as strings; an empty string removes a key's line.
- A tracker leaf missing from the tree, even one a known ticket's `after` names, comes back under `unmatched`: add an `[[entry]]` to the parent's `tickets.toml` (the folder's `next_id`, the item's type and title, a one-sentence description), mirror again with that `ref`, and put the item's body in the pulled file.
- Query published containers with their leaves; `mirror` skips them; tell the user when one's tracker status differs from its file's, and close one only through the closure check.
- A body that differs from the file: show both and ask.
- Move a ticket the tracker shows under another parent per Close, drop, move, then mirror again.
- Commit changed files on the current branch when `output_folder` is in a git repo; never push.

{{ workflow.board_after }}
