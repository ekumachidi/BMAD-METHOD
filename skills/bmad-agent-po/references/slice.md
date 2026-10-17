# Slicing

An initiative is sliced into epics; planning an epic writes all its stories. When the user has a split in mind, use it and say where the rules below would cut differently.

{{ workflow.tickets_before }}

## Every container, in order

1. **Envelope.** A document in the folder is an input, not the container; a missing container is written from its input per `spec.md`.
2. **Requirements.** Unwritten spec sections are written per The Operation in `spec.md`: from an input, or from the parent's lines when the epic only inherits.
3. **Complete the container**: known unknowns in Notes, Outcome and Done when re-read against Requirements and Constraints. When the container was just written, confirm its scope with the user before slicing.

## Read the code

On an existing codebase, read enough code to see where ownership and deployment split and which stories would touch the same code.

Where the code contradicts a Requirements line, add a `Source conflict:` line (the template's format) to the Notes of the container that owns the line, tell the user, and offer the correction through `spec.md`.

## One message, then revise

Before the draft is revised, one message holds what the spec and code show, each question that changes the split (first demo, least certain part, the team's epic rule) with your pick, and the draft in build order. Revise it in place until approved.

Offer to save a stated team rule as `slice_to_epics` (customize.toml).

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

Draft the set as the initiative's `tickets.toml` from `{{ workflow.tickets_template }}`: one `[[epic]]` per epic in build order, each with an `id`, a `description` of what is true when it is done, the parent ids it `covers`, and an `after` naming what it needs from which epic; `after` on an epic file only for a whole-epic gate. Run the tree check in `validate.md`, then present the draft in build order, naming the rule that cut it, with each epic's boundary, the tracer path, touch points with owners, and decisions several epics share.

On approval, create each new epic's folder and envelope from `{{ workflow.ticket_template }}`, with Requirements per `spec.md`, and commit (on a tracker, publish) per `board.md`.

Offer `bmad-architecture` to settle shared decisions in the architecture spine, cited in adopting epics' References. If declined, each becomes a `story` entry (`hitl = true`) in the opening epic's `tickets.toml`, written now, and each adopting epic gets `after = [{ epic = <opening>, needs = <the decision> }]`; planning keeps those entries first, ids kept.

Stop here unless the user wants to plan an epic's stories now.

## Epic into stories

After the epic file, read its breakdown, child tickets, and results. Each blocking unknown is settled now per `board.md`, recorded as the `unknown` of the entries it blocks, or spiked if the user asks.

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

Draft the breakdown as the epic's `tickets.toml` from `{{ workflow.tickets_template }}`, run the set check in `validate.md`, and revise it in place with the user until approved. Beyond the template:

- `after`: if this epic has an `after` in the initiative's breakdown, add the provider to each entry that needs it: `<epic id>.<entry id>` once that entry exists, else `epic-<slug>`.
- `refine = true` only on request.
- When the user says the epic will build unattended, ask for `plan_checkpoint` and `done_checkpoint` per entry, proposing `plan_checkpoint` on risky ones.
- Each touch point this epic owns is an entry or part of one.

With the draft, name the tracer bullet, parallel work, and deferred scope; record the tracer bullet as a dated `Decision:` line in the epic's Notes.

A split while planning adds an epic (folder, envelope, `[[epic]]` with its `after`) and moves its covers ids and entries; no file is renamed.

Write no leaf file until its entry is pulled.

Run `tickets.py status <initiative folder>`. Repoint each ticket in this epic's `blocks` (under `epics`) to the entry it waits for. Clear any `unpinned_after`, `undeclared_after`, or `order_conflict`, then commit (on a tracker, publish), both per `board.md`.

Tell the user the entries are ready to build; for `bmad-build-auto`, a loop, or a factory, recommend reviewing each entry now (`ticket.md`).

## Re-slicing

When completed work changes the picture, revise unstarted entries and tickets in place with the user, keeping each `id`; run the set check before approval, commit (on a tracker, publish) per `board.md`, and record the reason in the epic's Notes.

{{ workflow.tickets_after }}
