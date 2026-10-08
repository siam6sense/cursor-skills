# cursor-skills

Shared Cursor agent skills for Conventional Commits PRs and Jira story breakdown.

## Skills

| Skill | When to use |
| --- | --- |
| `create-pr` | Create/open a PR with Conventional Commits title, Jira trace, and a 200-line change budget |
| `jira-story-breakdown` | Break Jira stories into subtasks with fixed **1 / 2 / 3** catalog points |

## Install (Cursor, all projects)

```bash
npx skills add siam6sense/cursor-skills -g -a cursor -y
```

List without installing:

```bash
npx skills add siam6sense/cursor-skills --list
```

Update later:

```bash
npx skills update -g
```

After install, skills land in `~/.cursor/skills/` and apply across every project.

## Manual invoke

In Cursor Agent chat:

- `/create-pr`
- `/jira-story-breakdown`
