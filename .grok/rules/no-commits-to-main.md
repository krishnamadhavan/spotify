# Protected branch: `main`

`main` is a protected branch. **Never commit, amend, or push directly to `main`.**

## Required workflow

1. Create a branch from up-to-date `main` (`feat/…`, `fix/…`, `chore/…`, `docs/…`).
2. Commit only on that branch.
3. Open a PR into `main`. Merge via the PR (squash-merge is fine).

## Do not

- `git checkout main` then `git commit`
- `git push origin main` with local commits that are not already on `origin/main`
- Force-push `main`
- Merge locally into `main` and push

If the working tree is on `main` when a commit is needed, create and switch to a new branch first, then commit.
