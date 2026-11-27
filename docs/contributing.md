# Contributing to HireLens

Work is tracked in Jira project DP. Use the GitHub bug report or feature request
form for repository issues, and reference an existing Jira ticket where applicable
instead of creating duplicate tasks.

## Branches and pull requests

1. Branch from an up-to-date `main`, using a name such as `DP-123-upload-progress`.
2. Keep each commit focused on one change. Use
   `<TICKET>: [<SCOPE>] <type> <description>`, for example
   `DP-123: [frontend] feat show upload progress`.
3. Open a pull request into `main`. Use a draft while work is incomplete.
4. Name the PR `<type> hire-lens : <description>`, with `feat`, `fix`, or
   `refactor` as the type. Keep the ticket key in the description, not the title.
5. Write the description in Russian: a brief explanation and the ticket reference.
   The PR template provides these two fields; CI results are visible in Checks.
6. Get one approval from a code owner other than the author, pass both CI jobs,
   and resolve review conversations with the reviewer before merging.

The initial code owners are `@UlugbekBaxtiyorov` and `@mahmudcoding`, both existing
repository contributors with write access. The same pair covers the backend,
frontend, workflows, and documentation so either owner can contribute without
being the only person allowed to approve their own work. Ownership is defined in
`.github/CODEOWNERS`; update it by PR when team responsibilities change.

New code changes dismiss existing approvals. Add follow-up commits for review
fixes; do not amend or force-push an already published PR unless explicitly agreed.
Leave resolution of review comments to the reviewer or the person who requested it.

## Checks

Run these commands from the repository root:

```sh
uv run --locked --directory backend pytest
npm ci
npm run lint
npm run test:web
npm run build
```

GitHub Actions runs these checks for pull requests into `main`, pushes to `main`,
and manual runs. Required check names are:

- `Backend tests`
- `Frontend lint, tests and build`

Both jobs have a 15-minute timeout and a read-only repository token. PR checks do
not receive deployment credentials. Dependency changes must include the relevant
`backend/uv.lock` or `package-lock.json` update.

## Main branch rules

The `main` branch ruleset requires a pull request, one approving review, a code-owner
review, passing required checks against an up-to-date branch, and resolved review
conversations. It dismisses stale approvals and blocks force pushes and deletion.
There are no routine bypass actors, including the repository administrator.

CODEOWNERS and templates are repository files. Approval requirements and branch
protection must also be configured in **Settings → Rulesets**; adding the files
alone does not enable protection.

## Demonstrating the workflow

Open a small PR and verify that both CI jobs run. Confirm that merging is blocked
until checks pass and another code owner approves. If a test fails, fix it in a
new commit and obtain a fresh approval. The reviewer resolves their conversations.
After merging, the CI run on `main` checks the merged commit before any deployment.
