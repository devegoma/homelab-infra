# Issue tracker

Use the `gh` CLI for GitHub Issues in `devegoma/homelab-infra`. Pass `--repo devegoma/homelab-infra` to issue commands to target this repository explicitly.

- Publish an issue or spec with `gh issue create --title "..." --body-file <file>`.
- Fetch a ticket with `gh issue view <number> --json number,title,body,labels,comments`.
- List tickets with `gh issue list`, using `--state` and `--label` as needed.
- Add comments with `gh issue comment` and apply or remove labels with `gh issue edit --add-label` or `--remove-label`.
- Post the resolution with `gh issue comment` before closing a ticket with `gh issue close`.
- Link child tickets with GitHub sub-issues and record blockers with GitHub issue dependencies. If either feature is unavailable, use `Part of #<number>` or `Blocked by: #<number>` at the top of the child issue body. A ticket is unblocked when all its blockers are closed.

## Pull requests as a triage surface

**PRs as a request surface: no.**

## Wayfinding

Use one issue labelled `wayfinder:map` for the map and link its tickets as child issues. Use `wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, or `wayfinder:task` to identify each ticket's type.

In map order, select the first open child with no open blockers and no assignee. Assign it to yourself before starting. Record the answer in a comment, close the ticket, and add a brief decision and a link to the map.
