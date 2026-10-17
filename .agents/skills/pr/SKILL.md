---
name: pr
description: |
  Write a GitHub PR description from the branch's committed diff, then create or update the PR with `gh`.
  Use for /pr, "open a PR", "create/update the pull request", "write the PR description".
allowed-tools: Read, Bash, Write, AskUserQuestion
---

# PR

Source of truth: the commits on this branch.
Ignore `git status`, untracked and uncommitted files: the PR does not contain them.

Needs `gh`, authenticated (`gh auth status`). If missing, ask the user to set it up, then stop.

1. Collect the changes:

   ```bash
   git fetch origin
   base=$(git rev-parse --abbrev-ref origin/HEAD 2>/dev/null || echo origin/main)
   d=.tmp/agents/prs && mkdir -p $d
   git log --reverse --format='%h %s%n%b' $base..HEAD > $d/log.txt
   git diff $base...HEAD > $d/dump.diff
   echo "$d/pr_$(git rev-parse --short HEAD)_$(date +%Y%m%d%H%M%S).md"
   ```

   `...` diffs from the merge-base, like GitHub. Use the echoed path as-is.
   If `log.txt` is empty, tell the user there is nothing to PR, then stop.
2. Read `log.txt`, `dump.diff` and `.github/pull_request_template.md`.
3. Write the description to the echoed path:
   - Keep the template verbatim: same headings, checklist items, comments and order.
   - Tick `[x]` only the items that apply.
   - Fill sections concisely, only with what the diff shows.
     - Focus on rationale and impact.
     - Clearly, concisely articulate the problem space and solution space.
   - No template? Write a short summary.
4. Ask the user to review the file, and edit it if they want. Wait for their OK.
5. Propose a title with `AskUserQuestion`:
   - Option 1: a Conventional Commits title, under 70 chars.
   - Option 2: "I'll type my own title".
6. Run CI locally with `act`. If it fails, show the failed step, then stop.

   ```bash
   DOCKER_HOST=$(docker context inspect -f '{{.Endpoints.docker.Host}}') act pull_request
   ```

   `act` ignores the active Docker context, so set `DOCKER_HOST` from it.
   `act` tests the working tree (`--bind` in `.actrc`), not only the commits.
   If `git status --porcelain` lists changes, warn the user.
7. Push, then create or update the PR:

   ```bash
   git push -u origin HEAD
   [ "$(gh pr view --json state -q .state 2>/dev/null)" = OPEN ] && cmd=edit || cmd=create
   gh pr $cmd --title "<title>" --body-file <path>
   ```
