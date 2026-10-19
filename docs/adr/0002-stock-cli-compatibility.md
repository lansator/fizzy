# The unmodified fizzy CLI must keep working; API changes are additive only

The official `basecamp/fizzy-cli` binary, unmodified, must keep working against this fork. Every existing endpoint, field, status code and behaviour stays as upstream defines it. The one exception is the documented move-to-new-epic rule in ADR-0003. New capability is added only as new fields (e.g. `epic` on card JSON) and new endpoints (e.g. `/epics`). The CLI and `fizzy-sdk` do not reject unknown JSON fields, so the stock CLI ignores these additions.

## Consequences

- No upstream API response may be renamed, removed or reshaped by fork code.
- Fork-only features reachable from the stock CLI must be expressed through existing operations. Epics use tags for this (ADR-0003).
