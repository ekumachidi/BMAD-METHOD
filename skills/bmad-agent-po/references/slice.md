# Slicing

An initiative is sliced into epics: containers the product owner and developer own and complete. Inception plans the whole selected epic so AI agents can build it. The user's preference for a split comes first.

When an initiative has epics, every story is under an epic. Work that fits one epic is proposed as one epic; say so. Never offer an initiative without epics: it happens only at the user's request, is planned like an epic with stories directly underneath, and you say once that the initiative folder fills with ticket files.

{{ workflow.tickets_before }}

## Every container, in order

Apply this when authoring the initiative or incepting the selected epic.

1. **Envelope.** A document in the folder is an input, not the container. When the container file is missing, offer to create it from `{{ workflow.ticket_template }}`: title, a paragraph of intent, Outcome, Done when, boundaries, known decisions, and references. An epic also records its parent and the parent requirement ids it owns in `covers`. Done when is three to six checks at the altitude of the source's ids; it is the definition of the container and is never deferred.
2. **Requirements.** Write the spec sections per `spec.md` when they are not written yet; an epic that only inherits gets its lines from the parent ids `covers` names. The architecture spine, UX design, and research inform everything below either way.
3. **Complete the container** per `ticket.md`: known unknowns in Notes, and a re-read of Outcome and Done when against its Requirements and Constraints. The product owner and developer agree on its scope: propose the fields together with reasons, discuss what is unsettled rather than repeating decisions already made, and confirm before slicing.

## Learn the codebase and team first

Start from the parent chain: the parent ticket, its Requirements and companions, and what they reference, including the design when the work is user-facing. Then the codebase: greenfield or brownfield; mono or poly repo; team, service, and UI boundaries; vocabulary and recorded decisions — from the architecture document, else the repo layout. Then the areas the work will touch, enough to see which stories would change the same code. Tell the user what you read and concluded; ask what is wrong or missing and whether there are other references or tools you do not already know of.

Where the source contradicts the code or another source, add `Source conflict: <id or section> — <what the source says> vs <what was found>` to the Notes of the container whose source it is, and tell the user. Offer to log the correction through `spec.md`.

## Ask the questions that decide the split

Use what is already known. Ask the remaining questions that change the split: what is first worth demoing; what is least certain; what will the first piece teach about the rest; whether the user has a split in mind; how the team defines epics. Group related questions and say which answer you would pick and why.

When the user states how their team cuts epics, offer to save it as `slice_to_epics` under `[workflow]` in `{project-root}/_bmad/custom/bmad-agent-po.toml`.

Record each choice the user makes while slicing — a boundary, the tracer bullet, the sequencing, deferred scope, a declined suggestion — as a dated `Decision:` line in the container's Notes when it is made, not after approval.

## Initiative into epics

Existing epics are the working set: read them first and revise them in place, or drop one per `board.md` when it no longer fits; add only after the user confirms the set is insufficient.

{% if workflow.slice_to_epics %}
Draw the boundaries by the team's own rule:

{{ workflow.slice_to_epics }}
{% else %}
- An epic is one capability from the source, or a tightly coupled pair, delivered to production by one owner: a dev or pair with agent lanes.
- Propose epics along the source's capabilities. Merge two when one owner and one module deliver both. Two epics need at most a contract between them; say what each needs from the ones before it.
- A unit (module, service, or bounded context) is an epic boundary when it is also the ownership or deployment boundary and its Done when still reads as something a product owner can check. A module that is only a code folder is not.
- An epic whose boundary names more than one outcome or owner is two epics.
{% endif %}

Under either rule:

- No boundary applies: one epic. Split only for a distinct outcome, owner, or a part the user wants usable early, never for a ticket count.
- A unit the work only consumes or configures gets no epic: it is a touch point, named in the initiative's Boundaries with the epic that owns the work there.
- The platform baseline (scaffold, environments, CI, deployment, operations) is the opening epic, the first `[[epic]]` in the initiative's breakdown, or the first stories of the first epic.
- Every epic delivers to production; its Done when includes the integrated verification for what it delivers.

Draft the set, run the tree check in `validate.md` on the draft, then present it in recommended build order and say in one sentence which rule cut it. For each epic: title, one to three sentences of what is true when it is done, the parent ids it owns, its boundary, and what it needs from the epics before it. With the set, present:

- the tracer path across epics, when the first demo cuts through several;
- every touch point, with the epic that owns it;
- the decisions more than one epic must adopt: a contract, a message or data format, a shared value list. One repo or one unit has none.

Work with the user on order, boundaries, merges, splits, and anything unplaced.

On confirmation, write the initiative's `tickets.toml` from `{{ workflow.tickets_template }}`: one `[[epic]]` per epic in build order, each with an `id`, `after` naming what it needs and from which epic; `after` on an epic file only for a whole-epic gate. Each new epic gets its folder and envelope with Outcome and Done when, and its Requirements per `spec.md` from the parent ids it owns.

For the decisions more than one epic must adopt, offer `bmad-architecture` to settle them in the spine, and cite the spine section in each adopting epic's References. Declined: add each as a `story` entry (`hitl = true`) in the `tickets.toml` of the opening epic, and give each adopting epic `after = [{ epic = <opening>, needs = <the decision> }]`; its entries name that entry in `after` at their inception.

Stop at this level unless the user wants to incept an epic now; complete and plan only the selected epic. The others retain their scope, references, and place in the order without a breakdown.

## Epic into stories

The epic is ready to be worked and its Requirements are written. Read its breakdown, every child ticket, and their results before proposing changes. Plan the entire epic, not only its next story. Put each unknown that must be settled before implementation to the user: answer it now, record it as the `unknown` of the entries it affects, or add a spike when they ask for one. Never guess the design of work that depends on it.

What makes a good story:

- It is one implementation step toward the epic's Done when, small enough that one agent session, starting from the entry, its epic, and the source, plans and finishes it. It need not be user-visible on its own; the epic is the unit of value. Too small when setup outweighs the work: merge. An epic that is itself one session's work gets one story.
- {{ workflow.tickets_tracer_bullet }}
- A lane is a run of stories that touch the same code, in order; stories in different lanes never touch the same code. Two that would are one story, or one waits on the other. A boundary two lanes share gets its interface and a stub as its own early story, so both lanes open at once.
- Done is one runnable check. A story whose check needs another story's work goes after it.
- Split, never shrink: "for now", "placeholder", "simplified", "wired later" is a second story.
- Setup and each hitl step go on the first story that needs them, under the relevant epic and never loose under the initiative; initiative-wide setup belongs to the opening epic. Tests are part of every story; the one test story is the closing end-to-end suite below.
- {{ workflow.tickets_story_count }}

What opens and closes the breakdown:

- Offer an opening refactor when poor code quality or missing standards would make the epic's tickets hard.
- {{ workflow.tickets_refactor_sweep }}
- One closing end-to-end suite across the epic, after the sweep, is offered when the source has a test plan or the user wants one.

Draft one breakdown in build order, run the set check in `validate.md` on the draft, then present it, and adjust size, order, and prerequisites with the user until they approve the set. Each entry follows `{{ workflow.tickets_template }}` and has an `id` it keeps for good; the order of the tables is the build order. What the template does not say:

- `description` says what the entry delivers toward the ids in its `covers`, and `verify` how that result will be checked: one sentence each.
- `after` holds real prerequisites only, never the sequence. For each `after` on this epic in the initiative's breakdown, put the provider in `after` of every entry that needs it: `<epic id>.<entry id>` when the providing entry exists, `epic-<slug>` until it does.
- `refine = true` only where the user asks for full criteria on that entry; a bug gets them without it.
- `references` holds only what the entry needs beyond the epic's own References: a spine section, a design screen, a document. `notes` holds only what the user said, in their words.
- When the epic will run unattended, ask for `plan_checkpoint` and `done_checkpoint` per entry, proposing `plan_checkpoint` on a high-risk one.
- Each touch point this epic owns is an entry or part of one.

With the draft, name the tracer bullet, what can run in parallel, and any deferred scope. With the approval question, ask once whether the user wants to change a description or add a note or reference to any entry.

A split at inception is a second epic folder and envelope, a new `[[epic]]` in the initiative's breakdown with its `after`, the covers ids moved, and the agreed entries placed under the right epic; no file is renamed.

On approval, write the whole set into the epic's `tickets.toml`. No leaf file is written until its entry is pulled.

Then run `uv run {skill-root}/scripts/tickets.py --project-root {project-root} status <initiative folder>`. In `epics`, this epic's `blocks` lists the tickets in other epics whose `after` names the whole epic; replace each with the entry that delivers what it waits for. Clear any `unpinned_after`, `undeclared_after`, or `order_conflict` it reports as `board.md` says. Publication follows `board.md`.

Then tell the user the entries are ready to build, and what that means for how they build. With `bmad-build`, each ticket is refined during the build: the builder questions the user and writes the criteria itself, so refining here repeats that work. With `bmad-build-auto`, a loop, or a factory, nobody answers questions during the build and the entry is all the builder gets: recommend one more review of the sequence and of each entry now (`ticket.md`), and offer the checkpoints where none are set.

## Re-slicing

When completed work changes the picture, revisit the whole remaining breakdown with the user. Update unstarted entries and tickets in place, preserving each `id`; published changes go through the store. Do not rewrite completed or active work as a new breakdown. Run the set check on the revised draft before the user approves it, and record the reason in the epic's Notes.

{{ workflow.tickets_after }}
