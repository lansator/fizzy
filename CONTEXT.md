# Fizzy (personal fork)

A personal-task-management fork of 37signals' Fizzy, adding a cross-board view and a way to group cards into larger goals.

## Language

**Board**:
A container of cards with its own workflow columns; one per area of work.
_Synonym_: Project (interchangeable, both are fine)

**Card**:
A single task or idea that lives on exactly one board.
_Avoid_: Issue, ticket, task row

**Tag**:
An account-wide label attached to cards, shared across all boards.

### Card states

**Maybe?**:
The state of a published card not yet placed in a workflow column, i.e. awaiting triage.
_Avoid_: Stream, inbox, backlog

**Column**:
One step of a board's workflow that an open card sits in.

**Not Now**:
The state of a card set aside, by hand or automatically after a period of inactivity.
_Avoid_: Postponed, snoozed, archived

**Done**:
The state of a closed card.
_Avoid_: Closed, completed, resolved

### Epics and the Dashboard

**Epic**:
A larger outcome that groups cards from any number of boards; any **Tag** whose title starts with `epic:`.
_Avoid_: Goal, milestone, initiative

**Epic status**:
Where an **Epic** is in its life: _active_, _done_ or _dropped_; set by hand, independent of its cards.

**Target date**:
The day an **Epic** is meant to be finished by.
_Avoid_: Deadline, due date (cards keep those words)

**Progress**:
The share of an **Epic**'s cards that are closed; always counted, never set.

**Dashboard**:
A single view of cards from every board as rows in tables, grouped by **Epic**, by **Board**, or not at all.
_Avoid_: Overview, home, list view

**No epic**:
The **Dashboard** group holding cards that belong to no **Epic**.

## Relationships

- A **Card** belongs to exactly one **Board**
- A **Card** can carry zero or more **Tags**
- A **Tag** is shared across every **Board** in the account
- An **Epic** is a **Tag**, so it groups **Cards** across **Boards**
- A **Card** belongs to at most one **Epic**; giving it another **Epic** moves it there
- Changing an **Epic status** never opens or closes its **Cards**
- **Progress** counts every **Card** in the **Epic**, including those in **Not Now** and **Done**
- By default the **Dashboard** shows **Maybe?** and **Column** cards of _active_ **Epics** and **No epic**

## Example dialogue

> **Dev:** "When I quick-add a card in the q4-launch group on the **Dashboard**, where does it go?"
> **Alex:** "Onto whichever **Board** I pick, in **Maybe?**, and it joins the q4-launch **Epic**."
> **Dev:** "And if I later tag it epic:website?"
> **Alex:** "Then it moves to that **Epic**. A card has at most one."
> **Dev:** "If I mark q4-launch _done_ while two cards are still open?"
> **Alex:** "The **Epic status** changes, nothing else. The two cards stay open, and its **Progress** shows them."

## Flagged ambiguities

- "project" and "board" — resolved: the same concept, used interchangeably.
- "goals, milestones, epics" — resolved: one concept, **Epic**, single level.
- "closed" (code) vs "Done" (UI) — resolved: say **Done**.
