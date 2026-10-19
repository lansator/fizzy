# Epics are tags named `epic:<name>`

An **Epic** is any Tag whose title starts with `epic:`. It is not a separate table, and it is not a flag that only the web UI can set. Under ADR-0002 the stock CLI can then create epics (`fizzy card tag 42 epic:q4-launch`), list them (`fizzy tag list`) and filter by them (`fizzy card list --tag`) with no CLI changes. Tags are account-wide, so epics span boards for free. An epic's extra details (description, target date, status) are stored alongside its tag and edited in the web UI or through the new `/epics` endpoints.

## Consequences

- A card carries at most one epic tag. Adding a different epic tag removes the old one (a move), which is a deliberate change to plain tag behaviour. Tagging a card with the epic it already has still toggles it off, as upstream does.
- Renaming a tag into or out of the `epic:` prefix promotes or demotes it.
- The web UI shows epic names without the prefix.

## Considered Options

- **Separate `Epic` model with card `epic_id`**: cleanest model, but invisible to the stock CLI.
- **`kind` column on Tag set in the web UI**: the CLI can't create epics or tell them from plain tags.
