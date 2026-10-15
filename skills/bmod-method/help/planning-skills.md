# Planning skills in detail

Read this when the question is about Sofia (`bmad-agent-po`), `bmad-prd`, `bmad-ux`, or `bmad-architecture`: what each gives, when to pick it, when not to, and what it writes.

**`bmad-agent-po`** — Sofia the Product Owner. Talk to her to write a ticket's requirements from any input and slice from them; the ticket file is the spec at its level.
- Gives: the ticket's Requirements and Constraints, one line per requirement with a stable `R<n>` id naming its source (`R1 (FR-2, FR-5)`) or, on an epic, its parent's id (`R1 (parent R3)`), plus its Assumption and Open question lines and companions beside the ticket. She adopts UX files and an architecture spine as References lines and absorbs a PRD or brief as a source. She then slices: an initiative into epics, an epic into entries whose `covers` cite the `R<n>` ids. She also updates and validates the requirements of an existing ticket. The user reaches her by name ("ask Sofia", "the product owner") or by saying what they want ("distill this PRD", "slice this initiative", "what's ready"); no menu is shown.
- Pick when: the user has anything to distill, or an idea to work through with her; after any analysis or planning skill finishes, before architecture; when requirements change (she appends to the ticket's `.memlog.md`, re-derives the lines, and names the tickets that no longer match).
- Not when: the user wants to work a bare idea out first → `bmad-product-brief`, or `bmad-prd` when full requirements are needed. With sparse input she coaches too, walking the sections with the user.
- Writes: the ticket file, `<type>-<slug>.md` in its own `<type>-<slug>/` folder (the initiative's is `{output_folder}/{active_initiative}/`; epics sit under it), with `.memlog.md` and companions beside it, and `tickets.toml` beside each container.

**`bmad-prd`** — coaches detailed requirements out of the user.
- Gives: a PRD sized to the stakes (about 2 pages for a hobby project, longer for a launch): features, requirements with stable ids, user journeys, non-goals, MVP scope, metrics. Fast path or coaching path. Also updates and validates an existing PRD.
- Pick when: the user would rather work the idea out before it becomes the spec; a consumer or multi-stakeholder product; compliance, integration, or SLA concerns; an existing PRD needs editing or critique.
- Not when: scope is one or two stories → `bmad-build`. A lighter document will do → `bmad-product-brief`, then talk to Sofia the Product Owner to write the initiative from it. A brief is an optional input, never a prerequisite.
- Writes: `{output_folder}/{active_initiative}/prd-<slug>/prd-<slug>.md`.

**`bmad-ux`** — captures how the product looks and how it works. It may lead, follow, or stand alone.
- Gives: `DESIGN.md` (visual tokens and rules) and `EXPERIENCE.md` (structure, states, interactions, accessibility, key flows), optionally mockups and wireframes. It captures the user's vision and never imposes one. A design-handoff mode builds a prompt for an external design tool.
- Pick when: the UI is a significant part of the work; the user wants to design first and derive requirements from the design; the user has design assets to fold in; design will happen in an outside tool but a contract is still needed.
- Not when: there is no meaningful UI.
- UX first: its files are good input to `bmad-prd` when requirements still need drawing out, to `bmad-product-brief` when a lighter write-up will do, or talk to Sofia the Product Owner to write the initiative from them when the design already says enough.
- In the ticket: both files are adopted as References lines. Change them with `bmad-ux` update, not through the ticket.
- Writes: `{output_folder}/{active_initiative}/ux-<slug>/`: `DESIGN.md`, `EXPERIENCE.md`, and `ux-<slug>.md` naming them.

**`bmad-architecture`** — fixes only the decisions that keep separately built parts consistent.
- For a user new to architecture: it coaches by default, so the user needs no architecture knowledge to start. When the stack is open it recommends a well-known current starter, checked on the web first, because a good starter settles a coherent set of decisions for free. For each big call (paradigm, stack or starter, major boundaries, and where and how it is deployed and hosted) it lays out the realistic options and why it leans one way, then the user chooses. Its fast path drafts everything with `[ASSUMPTION]` tags to correct.
- Gives: a terse spine of decisions with stable ids, plus a list of what it deliberately leaves open. Not a full architecture document unless the user asks for one. Works at initiative, feature, or epic altitude, starting from the ticket's requirements; an existing codebase or a sprawling architecture document is input beside it.
- Pick when: two units built independently could choose incompatibly; an initiative has been cut into epics and more than one epic must adopt the same contract, format, or value list; the stack is open; the user does not know what stack, starter, or hosting to choose; a brownfield codebase has conventions worth ratifying; a feature touches an existing system.
- Not when: no initiative file exists yet → talk to Sofia the Product Owner to write it first; architecture reads the ticket. One session builds all of it → skip.
- Next: talk to Sofia the Product Owner to adopt the spine as a companion of the ticket; the epics cite it. Recommend that first.
- Writes: `{output_folder}/{active_initiative}/architecture-<slug>/architecture-<slug>.md`.

## Slicing and tracking the work

Sofia the Product Owner plans and tracks work in one ticket tree. An initiative holds epics; each epic's `tickets.toml` holds ordered entries. Build an entry directly without making a story file. Standalone stories and bugs can be direct intent or backlog leaves. Plans own status and remain live after completion. Builds stop at `built`; the user or orchestrator marks `done`. See `help/ticketing-setup.md`.

She slices initiatives into epics, incepts each epic into entries, refines when needed, and manages the board and optional tracker publishing. A PRD or described intent is valid input. Requirements stay in the epic and entries cite them with `covers`. A file is needed for refinement or tracker publishing, not to start a build. Tracker stores are lightly tested.
