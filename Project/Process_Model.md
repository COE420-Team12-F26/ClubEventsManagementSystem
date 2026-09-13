# Software Process Model

## Selected Model: Incremental Model

For the Club Events Management System, our team decided to go with the **Incremental process model**. Instead of trying to design and build the whole system in one shot, we're breaking it down into a series of smaller, working versions of the system, each one adding new functionality on top of the last.

Our planned increments (subject to adjustment as we go):

1. **Increment 1** – Core structure: user accounts, club/member roles, and basic event creation (no approval logic yet).
2. **Increment 2** – Event approval workflow: club officers submit events, advisors can approve/reject them.
3. **Increment 3** – Budget request module: officers submit budget requests tied to events, advisors approve/reject, running budget totals are tracked.
4. **Increment 4** – Notifications, dashboard summary, and general polish/testing.

## Why This Model Fits Our Project

We picked this model for a few reasons that are pretty specific to our situation:

- **The course is structured in weekly lab checkpoints**, so we already have to show incremental progress whether we like it or not. It makes sense to actually plan our development around that instead of fighting it.
- **We don't have 100% of the requirements nailed down yet.** We know the general shape of what club presidents, members, and advisors need, but details like exactly how the approval flow should work (single approver vs. multiple sign-offs, what happens on rejection, etc.) will probably get clearer once we start showing early versions to people and get feedback. Waterfall would force us to lock all of this down up front, which feels risky given how new we are to this problem space.
- **It lets us get something working early**, even if it's not the full system. Even after increment 1 we'd have a demoable product, which is reassuring for a course project where we're graded incrementally and want to avoid the classic "everything comes together in the last week" trap.
- Compared to something heavier like the Spiral model, our project is relatively small (3-person team, one semester, a fairly well-understood domain — clubs, budgets, approvals), so we don't need the level of formal risk analysis Spiral demands at every iteration. Incremental gives us most of the benefit (early delivery, flexibility) without that overhead.

## Overheads / Drawbacks and How We're Managing Them

Nothing comes for free, so here's what we're aware of:

- **Overhead 1 — Need for a solid architecture up front.** If we don't plan the overall structure (database schema, how modules talk to each other) reasonably well before increment 1, later increments can force painful rework. *How we're managing it:* before writing code, we're spending part of our design phase sketching the full data model (users, clubs, events, budgets) even though we're building it in pieces, so each increment slots into the same structure instead of contradicting it.
- **Overhead 2 — Integration effort repeats every increment.** Each time we add a piece, we have to re-test that it works with everything already built, which adds up over 4 increments. *How we're managing it:* we're keeping a small set of manual test cases (and basic unit tests where it's easy) that we re-run after every increment instead of only testing at the very end.
- **Overhead 3 — Scope creep between increments.** It's tempting to keep adding "just one more feature" to an increment instead of moving to the next one. *How we're managing it:* we're fixing what belongs in each increment before we start it (using a simple GitHub Projects/issues board) and pushing anything extra to a later increment or the out-of-scope list.
- **Overhead 4 — Splitting work across 3 people gets harder** when increments depend on each other (e.g., the budget module needs the event module to exist first). *How we're managing it:* we're assigning increments/features to whoever is most comfortable with that part (see Team/Skills.md) and using short weekly syncs to catch blocking issues early instead of finding out at the deadline.
