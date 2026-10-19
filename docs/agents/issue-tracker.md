# Issue tracker: GitHub

Issues and PRDs for this repo live as GitHub issues on the fork `lansator/fizzy`. Use the `gh` CLI for all operations.

## Fork caveat

This clone has two remotes: `origin` (`lansator/fizzy`) and `upstream` (`basecamp/fizzy`). Always target the fork: pass `-R lansator/fizzy` to every `gh issue …` command. Never open issues or PRs against `basecamp/fizzy`.

## Conventions

- **Create an issue**: `gh issue create -R lansator/fizzy --title "..." --body "..."`. Use a heredoc for multi-line bodies.
- **Read an issue**: `gh issue view -R lansator/fizzy <number> --comments`, filtering comments by `jq` and also fetching labels.
- **List issues**: `gh issue list -R lansator/fizzy --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'` with appropriate `--label` and `--state` filters.
- **Comment on an issue**: `gh issue comment -R lansator/fizzy <number> --body "..."`
- **Apply / remove labels**: `gh issue edit -R lansator/fizzy <number> --add-label "..."` / `--remove-label "..."`
- **Close**: `gh issue close -R lansator/fizzy <number> --comment "..."`

## When a skill says "publish to the issue tracker"

Create a GitHub issue on `lansator/fizzy`.

## When a skill says "fetch the relevant ticket"

Run `gh issue view -R lansator/fizzy <number> --comments`.
