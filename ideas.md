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
**Status:** shipped — on branch `feat/copy-name`, bound to `Y` `n`
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

**Built it.** `copy_name` action on `feat/copy-name`, bound to `n` in the copy
menu (`Y` `n` on the default keymap, and `n` in both the sane and vim presets),
plus `system:copy_name` for the right-click menu.

Registries that had to be touched — more than expected, worth remembering for
the next action I add:

| File | What |
|---|---|
| `action_buttons/copy_button.py` | `copy_name()` + `action_name()`, popup entry, key handler, click handler |
| `assets/keys.toml` | `[file_list.Y]` and `[copy_menu]` |
| `assets/presets/{sane,vim}.toml` | both, or preset users get no binding |
| `assets/config.toml` | `[keybinds.extra_copy]` default + right-click entry |
| `assets/schema.json` | action enum + keybind property |
| `classes/config.pyi` | generated stubs (`poe schema-to-dict` regenerates) |
| `core/file_list_right_click_menu.py` | `case "system:copy_name"` |
| `docs/.../{features/context-menu,reference/keybindings}.mdx` | ×2, `dev/` copies differ from released |

The right-click menu dispatches by *method name* — `file_list_right_click_menu.py:232`
does `hasattr(CopyButton, option.id)` — so `id="copy_name"` found `copy_name()`
with no extra wiring.

The keys system validates *contexts* (`variables/maps.py`), not action names, so
`copy.name` needed no registration; Textual resolves it to `action_name()` on
the widget holding `key_contexts = ("copy",)`.
