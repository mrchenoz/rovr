# Ideas

Things I want to build or change in my fork. Newest at the top.

<!--
Template to copy:

## Short title
**Status:** idea | exploring | building | shipped | dropped
**Why:** what problem this solves
**Notes:** links, file paths, gotchas
-->

## Copy just the filename, not the full path
**Status:** exploring — workaround found, proper fix worth writing
**Why:** `Y` `p` copies the whole path. Often I only want the bare filename.

**Finding: no filename-only copy exists.** All three copy actions deal in paths,
and the action set is a closed enum in `schema.json` — so it can't be added by
config alone:

| Action | Copies |
|---|---|
| `copy_highlighted` | full path of highlighted item |
| `copy_current_directory` | cwd |
| `copy_to_system_clip` | selected files, as file objects |

Implementation lives in `src/rovr/action_buttons/copy_button.py:99`.

**But the placeholder system already has it** (`src/rovr/functions/utils.py:360`):

| Placeholder | Expands to |
|---|---|
| `%h` | highlighted, full path |
| `%rh` | realpath of highlighted |
| `%nh` | **basename — just the filename** |
| `%rnh` | basename after resolving symlinks |
| `%cwd` `%rcwd` `%ncwd` | cwd variants |
| `%s` `%rs` | selected files, shell-joined |

Two constraints found the hard way:

- Placeholders only expand inside `run` actions, and `run` actions only exist in
  the `right_click` menu. The `keybinds` schema is a fixed set of named actions,
  so a custom command **cannot be bound directly to a key**. Reach it with
  <kbd>shift+F10</kbd> instead.
- `shell = true` is required for piping (schema default is `false`).
- **Gotcha:** the placeholder regex is `%[a-zA-Z_]+`, so it eats any `%word`.
  Don't write `printf %s` in the command — `%s` expands to the selected files.
  Use `echo -n` instead.

**Proper fix — good first upstream contribution.** Add a `copy_name` action
beside `copy_highlighted`, roughly:

```python
def copy_name(self) -> None:
    ...
    self.app.copy_to_clipboard(basename(highlighted.dir_entry.path))
```

Files to touch: `action_buttons/copy_button.py`, `assets/keys.toml`,
`assets/config.toml` (`keybinds.extra_copy`), `assets/schema.json` (action enum
+ keybinds), and the `right_click` default menu.
