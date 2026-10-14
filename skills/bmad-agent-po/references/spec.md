# Requirements

Turns any intent input into the ticket's **Requirements and Constraints sections and its `Assumption:` and `Open question:` lines**, plus its companions.

{{ workflow.spec_before }}

## Workspace

The workspace is the ticket the user or the calling capability names, else the active initiative; a ticket named by id or title resolves as The ticket tree in `workflow.md` says. A ticket with no file yet is created first from `{{ workflow.ticket_template }}` as `slice.md` step 1 says, non-goals to Boundaries, open questions to Notes. A new initiative is recorded as `active_initiative` as The ticket tree says; the request is the yes to creating it.

**No input.** Ask for a file path, pasted content, an explanation, or a source.

Inside the ticket's folder:

```
<type>-<slug>/
  <type>-<slug>.md   ← the ticket; Requirements, Constraints, Assumption: and Open question: lines are DERIVED from .memlog.md
  <companion>.md     ← optional
  .memlog.md
```

## Memory and derivation

`.memlog.md` is append-only, one line per entry; requirements carry their stable `R<n>`. Each run derives the Requirements and Constraints sections, the `Assumption:` and `Open question:` lines, and every spec-authored companion from the memlog plus the sources it cites.

SPEC is the only writer of those sections; a change goes to the memlog, never into them by hand. A derive replaces exactly those sections and lines, adds a References line per companion, and touches nothing else.

Writes go through `{project-root}/_bmad/scripts/memlog.py` (never read it back except to resume):

- `uv run {project-root}/_bmad/scripts/memlog.py init --workspace <ticket folder> --field topic="<the ticket's title>"` — once, at create.
- `uv run {project-root}/_bmad/scripts/memlog.py append --workspace <ticket folder> --type <decision|constraint|requirement|assumption|question|direction|note|event> --text "<one-line gist, reason included>"` — as each lands.
- Terminal moments (a validation verdict, "spec finalized") are `--type event` entries; the memlog has no status field.

## The Operation

Read the input and what it links. With a `.memlog.md`, this is an update: read the memlog, the authority on decisions and requirement ids; preserve them and give new requirements the next unused `R<n>`. A ticket with Requirements lines and no memlog is also an update: `memlog.py init`, then append each existing Requirements, Constraints, `Assumption:` and `Open question:` line as its own entry, ids kept. Otherwise this is a create. An epic whose only input is its parent's lines (the ids its `covers` names) gets one line per parent id, `R<n> (parent R<m>)`, in the parent's words, with no elicitation, self-validate or memlog; when the parent changes, its lines are re-rendered from the parent, ids kept.

Pre-sorted input (a PRD, a brief from an upstream BMad skill): keep its separation; lift each requirement and constraint into its line with the source's id in parentheses (`R1 (FR-2, FR-5)`), overflow into companions. Mixed input: sort each claim by the test in Load-bearing into a Requirements or Constraints line or a companion.

The template's comments say what Requirements and Constraints hold at this altitude. Rich input: extract directly. Sparse input: ask the user to choose **express** (every gap an `Open question:` line) or **guided** (walk the sections with the user one at a time).

An unaddressed domain implication (healthcare silent on PHI) becomes an `Open question:` line; never invent the answer. If these dominate, the input is too thin; suggest `bmad-prd`.

A later memlog entry supersedes an earlier one. When live sources disagree, or an either/or was never resolved, ask the user; the answer is a new memlog entry.

If the input is too thin to distill, stop and suggest `bmad-prd`. This capability distills; it does not coach.

## Load-bearing

A claim is **load-bearing** if any consumer (downstream skill, implementing agent, verification pass) would change a decision without it.

## Companions

Load-bearing content that needs more than one line (tables, diagrams, long reference material) goes in a companion; a line growing sub-bullets wants one.

- **Spec-authored** companions sit beside the ticket, named for their content; SPEC may edit them on update.
- **Adopted** companions (upstream artifacts such as `DESIGN.md`, pre-existing project docs) keep their names; SPEC lists them by relative path and never edits or duplicates them.

Diagrams always go in a companion; the ticket holds prose only.

## Spec Law

1. **Each Requirements line says what is possible and what shows it was met.**
2. **Requirements say WHAT, not HOW.** Implementation goes in a companion.
3. **Constraints bend design decisions.**
4. **Boundaries name at least one thing that is out.**
5. **The Outcome's signal is testable or demonstrable.**
6. **Requirement ids are never reused or renumbered.**
7. **Every load-bearing source claim lands in the ticket or a companion.**
8. **Every sentence in the ticket, companions, and `.memlog.md` carries load-bearing content.**

## Self-validate

{{ workflow.spec_self_validate }}

## When lines change

Offer to slice at most once per run, when the input reads as several independently shippable slices. Slicing runs per `slice.md`; load-bearing detail surfaced while slicing comes back here as an update, never into a child alone.

When an update changes or retires an id, name the stale entries and the tickets whose `covers` cite it, and offer to re-slice them per `slice.md`, Re-slicing; never edit a child.

## Output

In one or two sentences, share the ticket's path, requirement count, companions, and verdict. List any `Assumption:` or `Open question:` lines, one each, and invite the user to walk through them; they choose whether answers update the source, the ticket, or both. When the ticket is a container, offer to slice per `slice.md`, or to re-slice what went stale.

{{ workflow.spec_after }}

## After the sections are out

Append every later change to these sections to `.memlog.md`. When a change overrides a source input, offer to update that source too.

## References and sources

- A companion is a References line, `type — location`, the type naming its content; the location may be beside the ticket or outside it (`../ux-foo-bar/DESIGN.md`).
- `sources:` in the frontmatter lists files fully absorbed into Requirements, re-read on update; files downstream still needs go in References.
- Do not list the memlog, README files, organizational artifacts, or upstream process records.
