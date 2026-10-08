---
name: create-pr
description: >-
  Create pull requests with Conventional Commits titles, Jira traceability, and a
  hard 200-line DORA change budget. Use when the user asks to create a PR, open a
  PR, draft a PR, write a PR title or body, or run gh pr create.
---

# Create PR

Team/shared, project-agnostic playbook for opening pull requests.

Examples: [examples.md](examples.md)

## Hard rules

1. **Max 200 changed lines** (insertions + deletions) vs the PR base. Hard gate before `gh pr create`.
2. If size **> 200**, stop. Report the shortstat, suggest split slices, and do **not** open the PR unless the user explicitly overrides.
3. **Title** must be Conventional Commits + Jira key: `type(scope): imperative summary (PROJ-1234)`.
4. **Never invent a Jira key.** Extract from branch name, commits, or chat. If missing, ask. Omit the Jira section only when the user confirms there is no ticket.
5. Commit subjects on the branch should already follow Conventional Commits. When committing in the same flow and a Jira key exists, put `Refs PROJ-1234` in the commit body.
6. Return the PR URL when done.
7. Stricter project docs (e.g. EasyGig Elite &lt; 100) win when that project process applies — still run this skill’s checks, then apply the tighter budget.

## Title format

```
<type>(<optional-scope>): <imperative summary> (PROJ-1234)
```

- Types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`, `build`, `ci`, `style`, `revert`
- Imperative mood: add / fix / remove — not added / fixes / removing
- Prefer ≤72 characters; no trailing period
- No Jira key → ask first; only then use title without `(PROJ-1234)`

## Body template

Use this structure with `gh pr create` via HEREDOC:

```markdown
## Summary
- <1-3 bullets, why-focused>

## Jira
- Key: PROJ-1234
- Link: <browse URL if known>

## Test plan
- [ ] <checklist items>

## Size
- Changed lines vs <base>: N (budget 200)
```

Omit the **Jira** section only when the user confirmed no ticket.

## Workflow

Copy and track:

```
PR Progress:
- [ ] 1. Gather context
- [ ] 2. DORA preflight (≤200)
- [ ] 3. Resolve Jira key
- [ ] 4. Draft title + body
- [ ] 5. Push if needed
- [ ] 6. gh pr create
- [ ] 7. Return URL
```

### 1. Gather context (parallel)

- `git status`
- `git diff` and `git log` (branch vs base)
- `git diff --stat <base>...HEAD`
- Branch tracking / whether push is needed
- Infer base branch (`main`, `master`, `beta`, or repo default) from tracking and user intent

### 2. DORA preflight (hard gate)

```bash
git fetch origin <base>
git diff --shortstat <base>...HEAD
```

Parse insertions + deletions. If total **> 200**:

- Report N and the file stats
- Suggest how to split (by layer, file group, or subtask)
- **Stop** unless the user explicitly says to override

Project-specific tighter budgets (e.g. &lt; 100) still apply when documented for that repo.

### 3. Resolve Jira key

From branch patterns such as:

- `EG-1234/feature-name`
- `PROJ-1234-feature-name`
- `feature/PROJ-1234-...`

Else from recent commit messages / chat. If still missing → ask.

When a browse URL is available (user, project config, or Atlassian MCP), put it under **Jira → Link**.

### 4. Draft title + body

- Title: Conventional Commits + `(PROJ-1234)`
- Body: template above; Summary why-focused; Test plan concrete
- Size line must match the preflight number

### 5–6. Push and create

Follow the user’s PR creation rules (`gh`, HEREDOC body, push `-u` if needed). Example:

```bash
gh pr create --base <base> --title "feat(scope): short summary (PROJ-1234)" --body "$(cat <<'EOF'
## Summary
- ...

## Jira
- Key: PROJ-1234
- Link: https://...

## Test plan
- [ ] ...

## Size
- Changed lines vs <base>: N (budget 200)
EOF
)"
```

### 7. Return URL

Print the PR URL. Do not merge unless asked.

## Updating an existing PR

When the user asks to update title/body on an open PR, apply the same title, Jira, and body rules. Re-run the 200-line check against the PR base before claiming size is OK.
