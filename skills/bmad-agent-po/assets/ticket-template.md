---
tracker_id: ""   # with remote (the url), written at publish on a tracker; cut on the repo store. On a story, written by `tickets.py mirror`
key: ""   # container: tracker project or team for everything under this, when it differs from the store's
id: [story: the entry's id in tickets.toml, or the folder's `next_id` for a ticket with no entry; cut on a container]
type: [initiative | epic | story]
title: "[Container: the outcome this exists to reach. Story: what exists or works when this is done]"
parent: [folder name of the container above; none for an initiative or a standalone story in backlog/]
covers: [epic: the parent ids it owns; story: the epic's ids this ticket delivers toward; cut on an initiative, whose lines name their sources. A constraint is cited in References, not covered]
sources: []   # files SPEC fully absorbed into Requirements, for audit and re-read on update; downstream does not read these. Never the memlog
after: []   # container: epics this whole container waits on; the order of epics is in the initiative's tickets.toml. Story: prerequisites only: a sibling's id; "<epic id>.<entry id>" in another epic; epic-<slug> for that whole epic
assignee: ""   # container: status is added when work starts (in-progress | done | dropped). Story: a tracker's assignee, written by `tickets.py mirror`; otherwise assignee, blocked_at, and blocked_reason live in the plan
# a story's status lives in the plan beside this file, not here; tracker_status by a tracker sync
refined: false   # story only: true once the user approves its full criteria; a pulled story has this line only when its entry says `refine = true`
hitl: false   # story only
risk: [low|medium|high — on a container, the highest expected among its children]
---

<!-- One template at every altitude: the sections are the same and the comment on each says how its detail changes. A container at slicing is the envelope: frontmatter, Description, Outcome, Done when, Boundaries, References, known Notes; SPEC writes Requirements and Constraints, at the split from the parent's lines, from an input when one arrives. A pulled story: a one-sentence Description, Requirements as one Verify: line, References, local Notes. Reviewed: the same, gone over with the user. Refined, with numbered criteria and Boundaries: only a ticket with no epic, an entry with `refine = true`, or on request. A done or dropped ticket stays as it was written. -->

# [Title]

## Description

[Container: what is true when this is done and why it matters, one paragraph, pointing at the inputs (initiative) or the parent (epic). Story: one sentence, what this delivers, gone over with the user at review; with numbered criteria, 2–4 sentences on what exists or works when this is done and how it advances the epic; the criteria carry the proof, do not restate them.]

## Outcome

[Container: one sentence: for whom, what changes, and the signal that shows it worked — the source's, named not restated. Story: cut; the epic is the unit of value.]

## Requirements

[SPEC writes this section from the ticket's `.memlog.md`; slicing never writes a line here, and an edit goes through Sofia. Initiative: one line per requirement, `R<n> (<source ids>): <what, with the result someone can check>`, numbered from 1; the parentheses hold the source's ids, bare when they are unambiguous across the sources and prefixed with the source's References type otherwise (`prd FR-2`), or its section when it has none. Epic: the parent lines it owns, `R<n> (parent R<m>): <the line>`, in the parent's words, deepened only when inputs call for it; a line this epic splits or adds names its parent id or the source section the same way; an epic with empty `covers`, such as the platform baseline, names a source section on every line. Story: the acceptance criteria. One line, `Verify: how it will be checked`, nothing else. No epic, `refine = true`, or on request: the numbered criteria below instead, each naming the epic's ids it proves.]

1. **[Short name of the behavior]** (R<m>)
   **Given** [the state before: data, user, config]
   **When** [the one action]
   **Then** [what is observed: a response, a record, a screen — something a person or a test can check]
   **And** [a further observation from the same action, when there is one]

## Constraints

[SPEC writes this section. Initiative: each non-negotiable that bends design, one line, with its source id or section in parentheses; one that rules nothing out does not belong. Epic: the parent's constraints that bind this epic. Story: cut, unless this ticket adds one beyond its epic's; behavior that must stay as it is goes under Boundaries.]

## Done when

[Container: three to six checks the owner runs at the end, taken from the source's measures and limits; an initiative's read as business outcomes, an epic's as deliverables at the altitude of its Requirements, and every epic's include production delivery. Closing every child is not one. Story: cut; the `Verify:` line or the criteria in Requirements are the check.]

## Boundaries

[Container: which boundary it follows — team, service, UI, capability — and what it is not, from the source's non-goals. Initiative: then one line per unit the work touches that gets no epic, and the tracer path across epics in one sentence. Epic: what belongs to sibling epics. Story: with full criteria, adjacent behavior a valid implementation could damage — name behavior, not files; else cut.]

- Touch point: [unit] — [what is consumed or configured there]; owner: epic-[slug]
- Must not change: [behavior]

## References

[`type — location, section`: location is a path from {project-root}, or a url for a remote source. Every companion SPEC writes or adopts is a line here, typed by its content (glossary, design, architecture, prd), and downstream reads it with the ticket. Initiative: what this was distilled from. Epic: the parent, section Requirements, then the spine section and design it adopts. Story: the parent, and only what this ticket needs beyond the epic's own References.]

- parent — [path or url, section Requirements]
- [companion — a file beside this one, named for what it holds: glossary.md, state-machines.md]
- [design — an adopted companion, such as ../ux-<slug>/DESIGN.md, or a url for UI work]

## Notes

[What is not settled at this level, each line marked. Cut if empty. SPEC writes the Assumption: and Open question: lines from the memlog; the rest are slicing's and refining's.]

- Assumption: [a choice made without direct confirmation from the input or the user; confirmed, it becomes a dated Decision line]
- Open question: [container: what the source does not settle and which children wait on it. Story: what only this ticket waits on, answered as part of its work]
- Unknown: [what is not yet known and which entries wait on it, recorded now so it is not found late]
- Parked: [a requirement id not placed on any child, and why, with the user's knowledge]
- Decision: [a choice the user made, dated, so it is not asked again — a declined suggestion belongs here too]
- Source conflict: [id or section — what the source says vs what the code or another source shows]
- Waits on [epic] because: [the one-line reason for each entry in after]
- Dropped: [why, before status goes to dropped]

<!-- An initiative is the business outcome its epics serve, tied to a company goal, usually spanning quarters and more than one boundary. For a solo developer or small team with no larger goal above it, a whole product is a fine initiative.
Example, not part of the ticket: match its level of detail. Requirements holds the PRD's lines under this ticket's own ids, each naming the PRD section it came from; Constraints holds what binds every epic; Outcome names the PRD's signal. Done when reads as business outcomes a product owner checks at the end, not deliverables. The epic order, with what each needs from the one before, is in `tickets.toml` beside this file. -->

```markdown
---
type: initiative
title: "Checkout that shoppers finish"
parent: none
assignee: ""
risk: high
---

# Checkout that shoppers finish

## Description

Shoppers go from cart to paid order in one pass, with the price they saw, on any supported device or as a guest. The PRD owns the capabilities, constraints, and non-goals, distilled below; this initiative delivers them for the fall release.

## Outcome

The Q4 revenue target depends on lifting cart-to-payment completion from 55% to 70%; this initiative owns that number, the PRD's success signal, measured over a full quarter.

## Requirements

- R1 (PRD, Pricing): The price a shopper saw in the catalog is the price in the cart and at payment; a change in between is shown before they pay.
- R2 (PRD, Pricing): Every price on the site comes from one pricing call, so the catalog, the cart, and payment never disagree.
- R3 (PRD, Cart): A shopper adds, removes, and changes the quantity of any item; the total updates at once.
- R4 (PRD, Cart): A shopper applies one discount code; a refused code says why.
- R5 (PRD, Cart): The total shown is the total charged, before tax.
- R6 (PRD, Tax and payment): A signed-in or guest shopper pays the taxed total in one pass, on any supported device, and gets an order confirmation.

## Constraints

- Card data never touches our servers; PCI scope stays with the payment provider. (PRD, Constraints)
- Cart actions respond within 300 ms on the slowest supported device. (PRD, Constraints)

## Done when

1. Cart-to-payment completion holds at the PRD's target for one full quarter after release.
2. Every requirement is live for all shoppers, not behind a flag.
3. Refund and chargeback rates are no worse than the quarter before release.
4. No P0 or P1 checkout bug open for more than a day during the first month.

## Boundaries

The web store's checkout flow, the payment-provider integration, and guest checkout across web and mobile. Not the order-management backend beyond the order it creates, not loyalty, not the catalog; see the PRD's non-goals. Tracer path: one catalog item priced, carted, taxed, and paid by a signed-in shopper.

- Touch point: notification service — a new order-confirmation template, no code change; owner: epic-tax-and-payment

## References

- prd — _bmad-output/initiative-checkout/prd-checkout/prd-checkout.md, sections Capabilities and Constraints
- companion — glossary.md

## Notes

- Parked: gift cards (PRD, Gift cards); not in the fall release, user's call 2026-08-12.
- Decision: one payment provider for v1, replacing the current one (user's decision, 2026-08-12).
- Unknown: whether the tax service can meet the response-time constraint at peak; epic Tax and payment owns the answer.
```

<!-- Example, not part of the ticket: an incepted epic. The parent assigned R3–R5 to this epic from an unnumbered PRD; Requirements records those lines under this epic's own ids, each naming the parent id it maps to, and Constraints holds the parent constraint that binds it. Done when holds deliverable checks. Notes holds decisions and unknowns. -->

```markdown
---
type: epic
title: "Shoppers manage their cart"
parent: initiative-checkout
covers: [R3, R4, R5]
assignee: ""
risk: medium
---

# Shoppers manage their cart

## Description

A shopper adds items, changes quantities, applies discount codes, and recovers from every refusal with a reason — and the total they see is always the one the payment step receives.

## Outcome

Shoppers who reach the cart continue to payment more often because the total never surprises them; the PRD's cart-to-payment completion measure is the signal.

## Requirements

- R1 (parent R3): Add, remove, and change quantity of any item; the total updates at once.
- R2 (parent R4): Apply one discount code; refused codes say why.
- R3 (parent R5): The total shown is the total charged, before tax.

## Constraints

- Cart actions respond within 300 ms on the slowest supported device. (parent, Constraints)

## Done when

1. Every requirement works end to end on the live site, refusals included, with the reason shown.
2. The end-to-end suite proves the shown total equals the amount sent to payment on every cart path.
3. The 300 ms constraint holds on the slowest supported device under the load test.
4. A shopper with no code applied sees no change from today's cart.

## Boundaries

The cart UI. Not the pricing service (epic Pricing rules), not tax (epic Tax and payment).

## References

- prd — _bmad-output/initiative-checkout/prd-checkout/prd-checkout.md, sections Capabilities and Constraints
- design — https://figma.com/design/ab12cd/checkout, frame Cart

## Notes

- Decision: codes are case-insensitive (user's decision, 2026-08-12).
- Unknown: whether the discount engine can validate a code within the 300 ms constraint; entry 04 waits on it.
```

<!-- Example, not part of the ticket: match its level of detail. What is good here: the Description is what the shopper can do, end to end; every criterion states a rule, not an instance, with its failure path; Boundaries names behavior, not files; References points at the nearest document; Notes holds only what is not in the repo or the source, plus one assumption for the user to confirm. Numbered criteria because its entry says `refine = true`. -->

```markdown
---
id: 4
type: story
title: "A shopper applies a discount code and sees the new total"
parent: epic-cart-rules
covers: [R2, R3]
after: [3]
refined: true
hitl: false
risk: medium
---

# A shopper applies a discount code and sees the new total

## Description

A shopper with items in the cart enters a discount code, and the cart total updates to show the discount before tax. An invalid or expired code tells them why it was refused and leaves the total as it was.

## Requirements

1. **Valid code reduces the total** (R2)
   **Given** a cart and a valid discount code
   **When** the shopper applies it
   **Then** the total drops by the code's value and a discount line shows the code and the amount
2. **Expired code is refused with the reason** (R2)
   **Given** a cart and an expired code
   **When** the shopper applies it
   **Then** the total is unchanged and the message says the code expired and when
3. **Unknown code is refused without revealing valid codes** (R2)
   **Given** a cart and a code that does not exist
   **When** the shopper applies it
   **Then** the total is unchanged and the message says the code is not recognized, nothing more
4. **An applied code survives a quantity change** (R2, R3)
   **Given** an applied code
   **When** the shopper changes an item's quantity
   **Then** the total recomputes with the code still applied
5. **Payment receives the discounted total** (R3)
   **Given** an applied code
   **When** the shopper continues to payment
   **Then** the amount sent to payment equals the total shown

## Boundaries

- Must not change: catalog prices, tax computation, the cart for shoppers who apply no code.

## References

- parent — _bmad-output/initiative-checkout/epic-cart-rules/epic-cart-rules.md, Requirements R2 and R3
- design — https://figma.com/design/ab12cd/checkout, frame Cart

## Notes

- Decision: the discount engine's `validate(code, cart) -> {amount, reason}` interface is frozen (2026-08-12).
- Assumption: the refusal messages above are final copy; no design text exists for them.
```
