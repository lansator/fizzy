# Working a GitHub issue

How an agent session takes one issue from `lansator/fizzy` to a merged PR on
its own. Several sessions run in parallel, each in its own worktree, and merge
into `main` minutes apart. Most of the rules below exist because of that.

## 1. Set up the worktree

A new worktree has no database and no `tmp/` state. Run this first, from the
worktree root:

    bin/setup --skip-server

It needs mise 2026.8.10 or later and a trusted `.mise.toml` on the machine. If
it fails on either, stop and ask; don't work around it.

An agent's shell may not activate mise, so `ruby -v` can print the macOS
system Ruby instead of the one in `.ruby-version`. If it does, prefix every
Ruby command in this file with `mise exec --`, for example
`mise exec -- bin/ci`.

`tmp/saas.txt` must not exist. This fork is OSS-only, so the tests run on
SQLite.

## 2. Read before you build

- The issue, with `gh issue view -R lansator/fizzy <n> --comments`.
- `CONTEXT.md` and every ADR in `docs/adr/` that touches the issue. Use the
  glossary's terms in code, tests, commits and the PR.
- `STYLE.md`.

ADR-0001 keeps fork code in new files where it can, so upstream rebases stay
easy. ADR-0002 says the unmodified stock `fizzy` CLI must keep working: change
no existing API endpoint or JSON field, only add.

## 3. Build

Work test-first. Run one test file while iterating:

    bin/rails test test/models/card_test.rb
    PARALLEL_WORKERS=1 bin/rails test:system test/system/dashboard_test.rb

If the stock CLI check exists (`script/check_stock_cli`, from Issue-#2), run it
for any change that touches cards, tags, boards or the API.

## 4. The gates

    bin/ci

That is the only gate. GitHub Actions does not run tests for PRs opened inside
this fork, so nothing else will catch a red build before it lands on `main`.

Every step must pass except the last one, "Signoff". It calls the `gh signoff`
extension, which only posts a status to GitHub; if the extension isn't
installed, that step fails and the failure is not a gate failure. Say so in the
PR. Every other red step is a real failure.

Never loosen a check, skip a test or add a rubocop/brakeman exception to make a
gate pass. If you think a check is wrong, ask.

## 5. Open the PR

Open it against `main` on `lansator/fizzy`, never `basecamp/fizzy`. The body has:

- `Closes #<n>`.
- **Proven:** what the tests and gates show.
- **Not proven:** what nobody checked, such as a layout only seen at one width.
- **Decided:** every assumption or choice a human might disagree with.
- **Review:** filled in by step 6.

## 6. Review with Codex

Use the `codex` skill to review the diff only. Fix every objection that holds.
Answer the others in the PR, with the reason. Re-review after substantial
fixes, at most three rounds. Record each round's outcome under **Review**.

## 7. Merge

Only once the review is handled. In order:

1. `git fetch origin` then `git merge origin/main`. Never rebase or
   force-push: other sessions merge in parallel.
2. `bin/setup --skip-server` if the merge brought migrations, then `bin/ci`
   on the merged tree.
3. `git push`.
4. `gh pr merge --squash`. Delete the remote branch after you leave the
   worktree; it can't be deleted while the worktree uses it.

## 8. Report on the issue

Comment on the issue: what was proven, what was not, and any assumption you
made.

## Asking questions

Ask only when the answer changes the outcome and no safe default exists. Ask
the orchestrator session named in your prompt first, with SendMessage; it holds
the context of every issue in flight. Otherwise take the conservative option,
write it under **Decided**, and keep going.

When another session's work overlaps or blocks yours, tell the orchestrator
instead of building the same thing twice.
