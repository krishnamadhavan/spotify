# Conventional Commits

Always use [Conventional Commits](https://www.conventionalcommits.org/) for **git commit messages** and **PR titles** in this project.

## Format

```
<type>(<scope>): <description>
```

- Subject line only unless the body is needed to explain *why*
- Imperative mood: `add`, `fix`, `remove` — not `added` or `adds`
- Lowercase description, no trailing period
- Keep the subject ≤ 72 characters
- Scope is optional but preferred when the change is isolated

## Types

| Type | When |
|---|---|
| `feat` | New user-facing behavior |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting; no behavior change |
| `refactor` | Behavior-preserving code change |
| `perf` | Performance improvement |
| `test` | Tests only |
| `build` | Dependencies, toolchain, Docker |
| `ci` | CI configuration |
| `chore` | Maintenance that does not fit above |
| `revert` | Reverts a previous commit |

Breaking changes: append `!` after the type/scope (`feat(api)!: ...`) and add a `BREAKING CHANGE:` footer.

## Scopes (this repo)

Use when the change is mostly in one place:

`web` · `api` · `catalog` · `auth` · `player` · `docs` · `repo`

Omit the scope for cross-cutting changes.

## Examples

```
feat(player): persist queue position across refresh
fix(auth): send Origin on login CSRF check
docs: add v1 architecture to README
chore(repo): scaffold pnpm workspace and Django project
```

## PRs

PR title = the same conventional-commit subject as the primary change. Do not use `Update README` or `WIP` as the title.
