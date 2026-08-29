# Fork log

How my fork differs from upstream [NSPC911/rovr](https://github.com/NSPC911/rovr).

## Local patches I'm carrying

Changes of mine that are not upstream. Keeping this current is what makes each
sync predictable — anything listed here is what a merge could conflict on.

| Change | Branch / commit | Send upstream? |
|---|---|---|
| `copy_name` action — copy just the filename | `feat/copy-name` / `08d71f2` | yes, ready |
| `%rncwd` returns basename | `feat/copy-name` / `8bd7c7f` | yes, ready |

Both live on one branch as two clean commits, so either can be cherry-picked
into its own upstream PR.

## To send upstream

Fixes worth contributing back as a PR.

- **`%rncwd` placeholder is broken.** _Fixed in `8bd7c7f`, ready to send._
  `src/rovr/functions/utils.py:370` read
  `os.path.realpath(os.path.realpath(cwd))`. `realpath` is idempotent, so that is
  just `realpath(cwd)` — identical to `%rcwd`, with the `n` (basename) never
  applied. By the naming convention (`r` = realpath, `n` = basename, cf. `%rnh` =
  `basename(realpath(highlighted))`) it should be
  `os.path.basename(os.path.realpath(cwd))`. Small, self-contained fix.
- **No filename-only copy action.** _Built in `08d71f2`, ready to send._
  See `ideas.md` for the full list of registries an action has to be added to.

## Sync history

| Date | Synced to | Notes |
|---|---|---|
| 2026-08-29 | `eb4ffd2` | Fast-forward, 4 commits (chord keys #341 + app fixes) |

## Installing a branch

`uv` installs straight from git — no build step needed:

```sh
uv tool install --force --python 3.13 git+https://github.com/jeremy-oz/rovr@feat/copy-name
```

- `--force` is required: the branch keeps version `0.10.1.post1`, so uv sees no
  version bump and otherwise skips the reinstall.
- `--python 3.13` because `requires-python = ">=3.13,<3.15"`.
- `poe uv-build` normally generates `_schema_validator.py`, `COMMIT_HASH` and
  `DO_NOT_EDIT_THESE_FILES`. These are poe tasks, **not** PEP 517 build hooks, so
  a git install skips them. Both degrade gracefully — the schema validator falls
  back to `fastjsonschema.compile()` at runtime, and `--version` just omits the
  commit hash.

For actually hacking on the code, skip the install: `uv sync` then `uv run rovr`
(or `poe run`) picks up edits with no reinstall.
