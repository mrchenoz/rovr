# Planning notes

Personal planning notes for my [rovr](https://github.com/NSPC911/rovr) fork.

## Why there's no code here

This is an **orphan branch** — it has its own history and shares no commits with
`master`. That is deliberate:

- Syncing the fork from upstream can never conflict with these notes.
- Notes can never leak into a pull request sent upstream.
- `git log` here shows only note edits, not rovr's development history.

## Working with it

The comfortable way is a second worktree, so notes and code are both checked out
at the same time:

```sh
git worktree add ../rovr-notes notes
```

Notes then live in `../rovr-notes/` next to your normal checkout. Commit and push
from there as usual:

```sh
cd ../rovr-notes
git add -A && git commit -m "notes: ..." && git push
```

Remove the worktree whenever you like — the branch is untouched:

```sh
git worktree remove ../rovr-notes
```

Plain `git checkout notes` also works, but it swaps your single working tree over
and hides the source code while you're on it.

## Files

| File | Purpose |
|---|---|
| `cheatsheet.md` | Full rovr keybinding reference |
| `ideas.md` | Features and changes I want to make |
| `fork-log.md` | What I carry locally, and what to send upstream |
