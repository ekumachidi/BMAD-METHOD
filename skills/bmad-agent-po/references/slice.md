# Slicing

An initiative is sliced into epics; inception plans the whole selected epic. The user's own split comes first.

Every story sits under an epic unless the user asks for stories directly under the initiative.

{{ workflow.tickets_before }}

## Every container, in order

1. **Envelope.** A document in the folder is an input, not the container. Offer to create a missing one from `{{ workflow.ticket_template }}`; an epic records its parent and owned parent requirement ids in `covers`. Done when is three to six checks at the altitude of the source's ids.
2. **Requirements.** Unwritten spec sections are written per The Operation in `spec.md`: from an input, or from the parent's lines when the epic only inherits.
3. **Complete the container**: known unknowns in Notes, Outcome and Done when re-read against Requirements and Constraints. Confirm its scope with the user before slicing.

## Learn the codebase and team first

Read the parent chain, its references, and any design, then the architecture document (else the repo layout) and enough code to see its boundaries and which stories would touch the same code. Share your conclusions; ask what is wrong or missing and what other references or tools exist.

Where sources or code contradict, add `Source conflict: <id or section> — <what the source says> vs <what was found>` to the Notes of the container that owns the source, tell the user, and offer the correction through `spec.md`.

## Ask the questions that decide the split

Ask only open questions that change the split (first demo, least certain part, the user's split, the team's epic rule), with your recommendations.

When the user states how their team cuts epics, offer to save it as `slice_to_epics` under `[workflow]` in `{project-root}/_bmad/custom/bmad-agent-po.toml`.

Record each slicing choice, declined suggestions included, as a dated `Decision:` line in the container's Notes as it is made.

## Initiative into epics

Revise or drop existing epics per `board.md`; add only when the user confirms the need.

{% if workflow.slice_to_epics %}
Draw boundaries by the team's own rule:

{{ workflow.slice_to_epics }}
{% else %}
- An epic is one source capability, or a tightly coupled pair, that one owner delivers; two epics need at most a contract between them.
- A module, service, or bounded context is an epic boundary only when it is also the ownership or deployment boundary.
{% endif %}

Under either rule:

- One epic unless a distinct outcome, owner, or early-usable part splits it.
- A unit the work only consumes or configures is a touch point: list it in the initiative's Boundaries with its owning epic.
- The platform baseline (scaffold, CI, deployment) is the opening epic or the first stories of the first epic.
- Every epic delivers to production; its Done when includes integrated verification.

Draft the set, run the tree check in `validate.md`, and present it in build order, naming the rule that cut it. Per epic: title, what is true when done in one to three sentences, parent ids owned, boundary, needs from earlier epics. Add the tracer path, touch points with owners, and decisions several epics share.

On confirmation, write the initiative's `tickets.toml` from `{{ workflow.tickets_template }}`: one `[[epic]]` per epic in build order, each with an `id` and an `after` naming what it needs from which epic; `after` on an epic file only for a whole-epic gate. Create each new epic's folder and envelope, with Requirements per `spec.md`.

Offer `bmad-architecture` to settle shared decisions in the architecture spine, cited in adopting epics' References. If declined, each becomes a `story` entry (`hitl = true`) in the opening epic's `tickets.toml`, written now, and each adopting epic gets `after = [{ epic = <opening>, needs = <the decision> }]`; inception keeps those entries first, ids kept.

Stop here unless the user wants to incept an epic now.

## Epic into stories

Read its breakdown, child tickets, and results first. Each blocking unknown is answered now, recorded as the `unknown` of affected entries, or spiked if the user asks.

What makes a good story:

- One step toward the epic's Done when, sized for one agent session; it need not be user-visible. Too small when setup outweighs the work: merge. A one-session epic gets one story.
- {{ workflow.tickets_tracer_bullet }}
- Stories touching the same code form one ordered lane; an interface two lanes share gets a stub as its own early story.
- Done is one runnable check.
- Split, never shrink: "for now", "placeholder", "simplified", "wired later" is a second story.
- Setup and each hitl step go on the first story that needs them, initiative-wide setup in the opening epic. Tests go in every story; the only test story is the closing end-to-end suite.
- {{ workflow.tickets_story_count }}

What opens and closes the breakdown:

- Offer an opening refactor when code quality calls for one.
- {{ workflow.tickets_refactor_sweep }}
- Offer a closing end-to-end suite after the sweep if the source has a test plan or the user asks.

Draft the breakdown, run the set check in `validate.md`, and revise with the user until approved. Entries follow `{{ workflow.tickets_template }}`, keep their `id` for good, and sit in build order. Beyond the template:

- `description`: what the entry delivers toward its `covers` ids; `verify`: how it is checked; one sentence each.
- `after`: real prerequisites only. If this epic has an `after` in the initiative's breakdown, add the provider to each entry that needs it: `<epic id>.<entry id>` once that entry exists, else `epic-<slug>`.
- `refine = true` only where the user asks for full criteria; bugs get them anyway.
- `references`: only what the entry needs beyond the epic's References. `notes`: only the user's own words.
- For an unattended epic, ask for `plan_checkpoint` and `done_checkpoint` per entry, proposing `plan_checkpoint` on risky ones.
- Each touch point this epic owns is an entry or part of one.

With the draft, name the tracer bullet, parallel work, and deferred scope. At approval, offer once to edit any entry's description, notes, or references.

A split at inception adds an epic (folder, envelope, `[[epic]]` with its `after`) and moves its covers ids and entries; no file is renamed.

On approval, write the whole set into the epic's `tickets.toml`; write no leaf file until its entry is pulled.

Run `uv run {skill-root}/scripts/tickets.py --project-root {project-root} status <initiative folder>`. Repoint each ticket in this epic's `blocks` (under `epics`) to the entry it waits for. Clear any `unpinned_after`, `undeclared_after`, or `order_conflict`, then publish, both per `board.md`.

Tell the user the entries are ready to build; for `bmad-build-auto`, a loop, or a factory, recommend reviewing each entry now (`ticket.md`).

## Re-slicing

When completed work changes the picture, revise unstarted entries and tickets in place with the user, keeping each `id`; run the set check before approval, publish through the store, and record the reason in the epic's Notes.

{{ workflow.tickets_after }}
