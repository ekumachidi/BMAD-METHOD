# Validation checks

`validate.md` assigns sections to agents.

## Ticket

- References open; no unfilled placeholders; assumptions marked.
- Nothing contradicts the source, its companions, the architecture, or a recorded decision.
- Each entry has a description, `unknown` when one is known, and a `verify` a non-builder can run; only bugs, `refine = true` entries, and epic-less tickets need full criteria before execution.
- A ticket needing full criteria and covering an id in a `Source conflict:` line with no later `Decision:` has `refined: false`.

{% if workflow.checks.ticket %}
{{ workflow.checks.ticket }}
{% endif %}

## Set

Every entry in the epic's `tickets.toml` or draft breakdown, pulled or not.

- Tickets cover every Requirements line not deferred with the user; each `covers` id exists there; tickets sharing an id say which part each delivers.
- UX, architecture, constraint and integration work is covered without ids; results satisfy Done when; citing an id alone is not coverage.
- Every `after`, cross-epic too, is a real prerequisite, not restated order; once `tickets.toml` exists, `tickets.py status` on the epic shows no cycle, `drift`, `unpinned_after`, `undeclared_after`, or `order_conflict`; the epic's Notes hold the tracer bullet (the first entry) and sequencing decisions.
- A builder could write each story's acceptance criteria from the epic, entry, and references; if not, fix the epic, not the one-sentence `description` and `verify`.
- An epic of over three entries ends with a "Refactor sweep" story after all but a closing end-to-end suite, or a `Decision:` line says why not.

{% if workflow.checks.set %}
{{ workflow.checks.set }}
{% endif %}

## Tree

The initiative and its epic envelopes, drafted or written.

- Every initiative requirement has an accountable epic or agreed deferral; shared ones say which part each epic delivers; no unexplained overlap.
- Each epic's `covers` cites parent ids; each Requirements line names in parentheses its parent id, or its source section if the parent lacks it; a child's `covers` resolves through the Requirements line to a parent id.
- Every source id on a Requirements line exists in a `sources:` file, every parent id in the parent's Requirements; every adopted design or architecture document is in `companions:`.
- Every unit the source touches is an epic, out of scope, or a touch point (consumed or configured only, named in the initiative's Boundaries with its owner).
- Each decision several epics adopt has one home: an architecture spine section, or an opening-epic entry (first `[[epic]]`) the others wait on.
- Every epic envelope has Outcome, Done when (including production delivery), boundaries, upstream coverage, and references; a future epic needs no breakdown, its Requirements being the parent ids it owns; the initiative's `tickets.toml` lists every epic in build order, each `after` naming what it needs, opening with a platform-baseline epic or a `Decision:` line saying why not.
- With epics, no leaf sits directly under the initiative.

{% if workflow.checks.tree %}
{{ workflow.checks.tree }}
{% endif %}

## Dependencies

An item is a set entry or tree epic.

- Needs: what each item needs to start and to run its check is built by it or its `after`.
- Collisions: items with no dependency path share no code, config, schema, or setup; else add an `after` or merge them.
- Shared setup: the earliest item needing it owns it; others list it in `after`.
- Handoffs: when one item relies on another's output, both descriptions say so.

{% if workflow.checks.dependencies %}
{{ workflow.checks.dependencies }}
{% endif %}

## Closure

- Verify the container's Done when, requirements, constraints and companions against implementation evidence, not child status; explain dropped work and confirm remaining scope with the user.

{% if workflow.checks.closure %}
{{ workflow.checks.closure }}
{% endif %}
