# Fork log

How my fork differs from upstream [NSPC911/rovr](https://github.com/NSPC911/rovr).

## Local patches I'm carrying

Changes of mine that are not upstream. Keeping this current is what makes each
sync predictable — anything listed here is what a merge could conflict on.

| Change | Branch / commit | Send upstream? |
|---|---|---|
| _none yet_ | | |

## To send upstream

Fixes worth contributing back as a PR.

- **`%rncwd` placeholder looks broken.** `src/rovr/functions/utils.py:370` reads
  `os.path.realpath(os.path.realpath(cwd))`. `realpath` is idempotent, so that is
  just `realpath(cwd)` — identical to `%rcwd`, with the `n` (basename) never
  applied. By the naming convention (`r` = realpath, `n` = basename, cf. `%rnh` =
  `basename(realpath(highlighted))`) it should be
  `os.path.basename(os.path.realpath(cwd))`. Small, self-contained fix.
- **No filename-only copy action.** See `ideas.md` — `%nh` exists for custom
  commands, but there is no `copy_name` built-in to sit next to
  `copy_highlighted`.

## Sync history

| Date | Synced to | Notes |
|---|---|---|
| 2026-08-29 | `eb4ffd2` | Fast-forward, 4 commits (chord keys #341 + app fixes) |
