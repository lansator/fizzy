# Fork Fizzy in Rails instead of rewriting it in TypeScript

This is a personal fork of `basecamp/fizzy` that adds a cross-board **Dashboard** and **Epics**. We keep the upstream Rails app and change it, rather than reimplementing it in TypeScript. The two missing features are a new view over an existing query (`Filter#cards`) plus an additive model change. The hard constraint in ADR-0002 would force a rewrite to reproduce the whole API contract (112 operations in `basecamp/fizzy-sdk`'s `openapi.json`) and chase every CLI release.

## Consequences

- Upstream moves fast (about 225 commits in 90 days, touching nearly every file under `app/`). Fork changes go in **new files** wherever possible: new controllers, views, models, CSS and migrations. Edits to upstream files stay minimal and known, e.g. the root route in `config/routes.rb`. That keeps `git merge upstream/main` cheap.
- We inherit upstream's stack: Rails main branch, Hotwire, SQLite by default.

## Considered Options

- **TypeScript rewrite scoped to one user**: rejected once stock-CLI compatibility became a requirement (ADR-0002).
- **Faithful TypeScript port**: rejected. It is weeks of review to reproduce a product whose model we only partly want.
