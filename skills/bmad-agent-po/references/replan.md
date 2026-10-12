# Replanning

A change has arrived while work is under way: a technical limit found while building, a new requirement, a misreading of the source, a pivot, a failed approach. The outcome is a tree whose remaining tickets are true again, with the reason on record, and nothing rewritten that is already built. No proposal file.

{{ workflow.replan_before }}

1. **Name the change.** One or two sentences: what changed, where it came from, and what shows it. Resolve the ticket it lands on with `tickets.py find`; read it, its `.memlog.md`, its breakdown and the finished children, and the architecture or design when the change touches them; ask only what the tree cannot tell you. Say whether what is required changed or only how it is delivered, and which way forward you propose — adjust the remaining entries, roll back built work, or cut scope — with the reason. The user decides.
2. **What is required.** When the change alters what the ticket requires, run `spec.md` on it with the change as the input: the lines re-render, ids kept, and the entries whose `covers` cite a changed id are named. When only the how changed, say so and skip this step.
3. **Re-slice** each affected epic per Re-slicing in `slice.md`; an epic made obsolete, or a new one needed, is a change to the initiative's breakdown per Initiative into epics there.
4. **Drop** what no longer belongs per Close, drop, move in `board.md`, and clear what waited on it.
5. **Record the reason** as a dated `Decision:` line in the Notes of each container the change touched, in the words from step 1. A change that invalidates the architecture spine or the design gets a line there too, with the offer to invoke `bmad-architecture` or `bmad-ux`.

Then say what changed, what was dropped, and what is ready now.

{{ workflow.replan_after }}
