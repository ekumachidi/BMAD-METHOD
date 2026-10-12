# Requirements

Takes any intent input — vague idea, brain dump, PRD, GDD, RFC, brief, Slack thread, customer email, meeting transcript, mockups, mixed multi-source — and produces the ticket's **Requirements and Constraints sections and its `Assumption:` and `Open question:` lines**, plus companion files for load-bearing content that does not fit or would bloat a line with expansive line-item detail. Together they are the machine contract every downstream BMad skill consumes.

Other capabilities and skills update the same ticket over time through this one.

{{ workflow.spec_before }}

## Workspace

The workspace is the ticket the user or the calling capability names, else the active root (The ticket tree in `workflow.md`). Its folder holds the ticket file, `.memlog.md`, and the companions. A ticket named by id or title resolves as The ticket tree says. A ticket with no file yet — a new initiative from an input, or a standalone story from a one-session ask — is created first from `{{ workflow.ticket_template }}` as `slice.md` step 1 says, its envelope filled from the input: the non-goals go to Boundaries, the open questions to Notes. A new initiative is recorded as `active_initiative` as The ticket tree in `workflow.md` says; the request is the yes to creating it.

**No input.** Ask the user to share a file path, paste content, explain the idea in detail, or point to a source.

Inside the ticket's folder:

```
<type>-<slug>/
  <type>-<slug>.md         ← the ticket; its Requirements, Constraints, Assumption: and Open question: lines are DERIVED from .memlog.md, never hand-edited; its other sections are the ticket's own
  <companion-1>.md         ← optional, content-typed (e.g. glossary.md); spec-authored ones are derived too
  <companion-2>.md
  .memlog.md               ← canonical, append-only memory; what those sections are distilled from
```

## Memory and derivation

`.memlog.md` is canonical — an append-only, chronological record of every decision, constraint, requirement (with its stable `R<n>`), assumption, open question, and bit of user direction, one line each in the order it happened, never edited or reordered. The ticket's Requirements and Constraints sections, its `Assumption:` and `Open question:` lines, and every spec-authored companion are **derived on each run** from the memlog (the decision-of-record) plus the sources it cites for raw content — never hand-patched.

Deriving the contract from a living log instead of editing the contract in place is what lets the steps around the ticket (PRD, UX, architecture, epics) run in any order and feed the same ticket without merge drift: the log only accumulates, the sections are re-rendered. So those sections are updated *only* by re-deriving them here — SPEC is their single writer, and slicing never writes a requirement line; a hand-edit to one of them is overwritten on the next derive, so a change goes to the memlog. A derive replaces exactly those sections and lines, adds a References line for each companion, and touches nothing else in the file: the rest of the ticket is slicing's and refining's, per `ticket.md`.

Writes go through the shared script — `{project-root}/_bmad/scripts/memlog.py`, the same location as `resolve_customization.py` (atomic; never read it back except to resume):

- `uv run {project-root}/_bmad/scripts/memlog.py init --workspace <ticket folder> --field topic="<the ticket's title>"` — once, at create.
- `uv run {project-root}/_bmad/scripts/memlog.py append --workspace <ticket folder> --type <decision|constraint|requirement|assumption|question|direction|note|event> --text "<one-line gist, reason included>"` — as each lands.
- Terminal moments (a validation verdict, "spec finalized") are `--type event` entries; the memlog carries no status field.

## The Operation

Read the input and its ancillary linked materials. If there is no input, follow the no-input branch in **Workspace** (ask). If a prior `.memlog.md` exists at the ticket's folder, read it — the operation becomes an update, and the memlog (not the rendered ticket) is the authority on what was decided and on requirement ids. Preserve those ids; new requirements get the next unused `R<n>`; never reuse retired ids. A ticket with Requirements lines and no memlog is also an update: `memlog.py init`, then append each existing Requirements, Constraints, `Assumption:` and `Open question:` line as its own entry, ids kept, before reading the input. Otherwise this is a create, and the first move is `memlog.py init`. An epic whose only input is its parent's lines — the parent ids its `covers` names — gets one line per parent id, `R<n> (parent R<m>)`, in the parent's words, with no elicitation, no self-validate pass and no memlog; it deepens on a later run when an input calls for it.

When the input is structured and pre-sorted (a PRD with an addendum, a GDD, a brief produced by an upstream BMad skill), trust the authored separation: lift each requirement and constraint into its line, the source's id in parentheses (`R1 (FR-2, FR-5)`), and lift overflow into appropriately-named companions. When the input is mixed (a brain dump, a transcript, an RFC, a customer email), do the sorting yourself: walk each claim, apply the three-lens load-bearing test (Spec Law rule 7), and route to a Requirements or Constraints line or a companion.

Distill the input into the ticket's Requirements and Constraints; the template's comments say what each holds at this altitude. When input is rich, extract directly — no elicitation. When input is sparse, choose: **express** (best-effort distill, every gap becomes an `Open question:` line) or **guided** (walk the sections with the user one at a time). Ask which.

A recognized domain implication the input leaves unaddressed *is* such a gap — name it as an `Open question:` line (healthcare input silent on PHI/HIPAA, payments silent on PCI, control systems silent on fail-safe) and move on. Flag it; never invent the answer or coach toward it. If these dominate, the input is too thin — suggest `bmad-prd`.

Write lean from the first pass: every sentence must earn its place. Decoration costs tokens and dilutes downstream readers.

Log each decision, requirement, constraint, and accepted change to `.memlog.md` as it is made — that running record is what the render reads. Because the log is append-only, a later entry supersedes an earlier one on the same point while the history stays intact. When two currently-live sources or companions disagree on the same field, or an either/or never got resolved, surface it to the user rather than silently choosing — the resolution is itself a new memlog entry.

If the input is genuinely too thin to distill (e.g. "an app for hikers" with no surrounding context), stop and suggest `bmad-prd` (or sibling ceremony skill). This capability distills; it does not coach.

## Load-bearing

A claim is **load-bearing** if any consumer (downstream skill, implementing agent, verification pass) would change a decision without it.

## Companions

When load-bearing content does not fit a Requirements or Constraints line, it lives in a companion. The ticket cites it; the companion holds it. Companions are part of the contract; every consumer finds them in the ticket's References. Companions follow the same lean discipline as the ticket (Spec Law rule 8).

**Spawn a companion when the content needs more than one line:** multi-item catalogs (per-entity matrices like archetypes, drinks, modes, routes), tables, diagrams (always), editorial voice rules, long-form reference material a line cites by name (glossary, brownfield notes, project conventions). Single-line decision-benders stay in Constraints; what-and-check pairs stay in Requirements. If a line is starting to bullet into sub-bullets, the content has outgrown the section and wants a companion.

Companions are either:

- **Spec-authored** companions are written by SPEC and live **beside the ticket** (e.g., `glossary.md`, `patron-archetypes.md`). SPEC owns them and may edit them on update operations.
- **Adopted** companions are load-bearing artifacts written by an upstream skill that downstream still needs to read. SPEC lists them in References by relative path but does NOT edit them (e.g., a `DESIGN.md` or `EXPERIENCE.md` from a UX run, an integration partner's API spec). The originating skill owns them.

Two rules govern companions:

1. **Name spec-authored companions for the content type they hold.** `glossary.md`, `<entity-class>.md` (e.g. `patron-archetypes.md`, `medication-routes.md`, `flight-modes.md`), `stack.md`, `conventions.md`, `brownfield.md`, `architecture-diagrams.md`, `state-machines.md`, `failure-modes.md`, `compliance-references.md`. The principle: "a reader should know what is inside before opening it." Adopted companions keep whatever name their originating skill gave them.
2. **Diagrams always land in a companion**, regardless of size. The ticket holds prose only. Mermaid blocks, ASCII diagrams, and image references all live in a companion (e.g. `architecture-diagrams.md`), with sibling image files referenced from there.

Pre-existing project-wide docs (e.g. `project-context.md`) that downstream needs are listed as **adopted companions**, never duplicated into the ticket or a spec-authored companion.

## Spec Law

Every ticket must satisfy these eight rules. The operation aims for them; the self-validate sweep enforces them.

1. **Each Requirements line says what is possible and what shows it was met.** Missing either = not a requirement.
2. **Requirements describe WHAT, not HOW.** Implementation prescription belongs in a companion (stack, conventions).
3. **Constraints actually bend design decisions.** A "constraint" that rules nothing out is decoration.
4. **Boundaries say what is out.** At least one line. Absence means downstream skills fill the vacuum.
5. **The Outcome's signal is concrete enough to test or demonstrate against.** "Users love it" doesn't qualify.
6. **Requirement ids are stable and unique.** Never reused, never renumbered.
7. **Preservation.** Every load-bearing source claim lands in the ticket or a companion. Wrapper ceremony does not.
8. **Lean prose.** Every sentence carries load-bearing content. Cut decoration, hedges, backstory, throat-clearing. Applies to the ticket, companions, and `.memlog.md`.

## Self-validate

{{ workflow.spec_self_validate }}

## When lines change

Offer to slice at most once per run, when the input reads as multiple independently shippable slices; a decline ends the offer for this run, not forever. Slicing runs per `slice.md`: the children's Requirements are cut from this ticket's lines, and load-bearing detail the slicing conversation surfaces (a constraint, a design decision) comes back here as an update, never into a child alone.

When an update changes or retires an id, name the entries and tickets whose `covers` cite it (search the breakdowns under the ticket) and offer to re-slice them per `slice.md`, Re-slicing. The update itself never edits a child.

## Output

Share the ticket's path conversationally. Name the requirement count, the companions produced, and the verdict in one or two sentences. If there are `Assumption:` or `Open question:` lines, list them (short — one line each) and invite the user to walk through them. Make clear that addressing them can update the source input (if it was a file), the ticket, or both — whichever combination the user prefers. Do not dump JSON or present a wall of output. When the ticket is a container, offer to slice per `slice.md`, or to re-slice what went stale.

{{ workflow.spec_after }}

## After the sections are out

Any update to these sections — resolved assumptions, answered open questions, other changes — is appended to `.memlog.md` as it happens. When a change overrides something that came from a source input, offer to update that source too, so upstream and the ticket don't silently diverge.

## References and sources

- A companion is a line in the ticket's References, `type — location`, the type naming its content: paths may point beside the ticket (spec-authored, `glossary.md`) or outside it (adopted, `../ux-foo-bar/DESIGN.md`); downstream treats both the same.
- `sources:` in the ticket's frontmatter lists the files that were **fully absorbed** into Requirements, with no remaining downstream value (e.g., a PRD whose every load-bearing claim is now a line). Listed for audit and for re-reading on update. Downstream does NOT read these. Files downstream still needs belong in References, not here.
- **Do not list** the memlog, README files, organizational artifacts, or any operational record of how upstream skills produced their artifacts. Those are not source content; they are process metadata that downstream consumers don't need.
