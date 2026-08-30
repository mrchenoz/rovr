# rovr cheat sheet

The **default** keymap, transcribed from `src/rovr/assets/keys.toml` at upstream
`eb4ffd2`. Keys marked 🔧 are my fork's additions and are not upstream.

> **Press `?` inside rovr** for the live keybind list — it always reflects your
> actual config, including anything you have rebound. This sheet is for reading
> away from the terminal.

## Why the same key does different things

rovr resolves a key from the focused widget **outward**, and the first match
wins. Specific contexts beat general ones — `file_list` is checked before
`lists`, `search_input` before `inputs`. `global` applies everywhere, even over
a modal. `main` applies only while the main screen is unobstructed.

That is why `j`/`k` move the cursor in a list but scroll the preview when the
preview pane has focus. Same keys, different context.

---

## Global — works anywhere, modals included

| Key | Action |
|---|---|
| `q` | Quit, cd-ing to the current directory |
| `Q` | Quit **without** cd-ing |
| `ctrl+q` | Quit |
| `ctrl+p` | Command palette |
| `ctrl+z` | Suspend rovr |

## Main screen

### Panels

| Key | Action |
|---|---|
| `S` | Toggle pinned sidebar |
| `I` | Toggle preview sidebar |
| `F` | Toggle footer |
| `M` | Toggle menu |
| `s` | Focus pinned sidebar |
| `i` | Focus preview sidebar |
| `c` | Focus clipboard panel |
| `m` | Focus metadata panel |
| `#` | Focus processes panel |
| `ctrl+l` | Focus path switcher |
| `escape` | Focus file list |

Uppercase toggles the panel, lowercase focuses it.

### Tabs

| Key | Action |
|---|---|
| `n` | New tab |
| `w` | Close tab |
| `ctrl+j` | Next tab |
| `ctrl+k` | Previous tab |

### Tools

| Key | Action |
|---|---|
| `?` | Show keybindings |
| `>` | Open shell |
| `z` | Jump to a directory (zoxide) |
| `f` | Search for files (fd) |
| `\` | Search file contents (ripgrep) |

---

## File list

### Moving

| Key | Action |
|---|---|
| `j` · `down` | Down |
| `k` · `up` | Up |
| `l` · `right` · `enter` | Enter directory / open |
| `h` · `left` | Parent directory |
| `H` | Up through single-child folders |
| `L` | Down through single-child folders |
| `g` · `home` | First item |
| `G` · `end` | Last item |
| `ctrl+f` · `pagedown` | Page down |
| `ctrl+b` · `pageup` | Page up |
| `u` · `backspace` · `alt+left` | Previous directory (history) |
| `alt+right` | Next directory (history) |
| `/` | Search the file list |

`u` goes **back** through history, like vim's undo — not "up a directory".
That's `h`. History-forward has no single-letter binding, only `alt+right`.

### Selecting

| Key | Action |
|---|---|
| `v` | Toggle visual selection |
| `insert` | Toggle the current item |
| `%` · `ctrl+a` | Toggle all |
| `J` · `shift+down` | Extend selection down |
| `K` · `shift+up` | Extend selection up |
| `shift+pagedown` | Extend one page down |
| `shift+pageup` | Extend one page up |
| `shift+home` | Extend to first |
| `shift+end` | Extend to last |

### Acting on files

| Key | Action |
|---|---|
| `y` · `ctrl+c` | Copy |
| `x` · `ctrl+x` | Cut |
| `p` · `ctrl+v` | Paste |
| `d` · `delete` | Delete |
| `r` · `f2` | Rename |
| `a` · `ctrl+n` | New file or directory |
| `A` · `N` | Bulk create |
| `E` | Create archive |
| `ctrl+e` | Extract archive |
| `o` | Open |
| `e` | Open in editor |
| `P` | Toggle pin |
| `.` | Toggle hidden files |
| `shift+f10` | Context menu |

---

## Chord menus

Press the leader, then the second key. Both open a visible menu, so you can
pause and read it.

### `Y` — copy

| Keys | Action |
|---|---|
| `Y` `y` | Copy files to rovr's clipboard |
| `Y` `p` | Copy highlighted **path** |
| `Y` `n` 🔧 | Copy highlighted **name** |
| `Y` `s` | Copy files to the system clipboard |
| `Y` `d` | Copy current directory path |

### `,` — sort order

| Keys | Action |
|---|---|
| `,` `a` | By name |
| `,` `e` | By extension |
| `,` `n` | Natural |
| `,` `s` | By size |
| `,` `c` | By created time |
| `,` `m` | By modified time |
| `,` `d` | Toggle descending |
| `,` `p` | Toggle sorting for this path only |

---

## Lists — anywhere a list has focus

Inherited by the file list, pinned sidebar, clipboard, search results, popups
and menus.

| Key | Action |
|---|---|
| `j` · `down` | Down |
| `k` · `up` | Up |
| `l` · `right` · `enter` | Select |
| `g` · `home` | First |
| `G` · `end` | Last |
| `ctrl+f` · `pagedown` | Page down |
| `ctrl+b` · `pageup` | Page up |

## Preview pane and scrollable views

Same movement keys — `j` `k` `g` `G` `ctrl+f` `ctrl+b` and the arrow/page/home/
end equivalents — scrolling rather than moving a cursor.

---

## Modal prompts

### Delete

| Key | Action |
|---|---|
| `d` | Move to trash |
| `x` | Delete permanently |
| `c` · `escape` | Cancel |

### Trash

| Key | Action |
|---|---|
| `r` | Restore |
| `x` | Purge |
| `E` | Empty trash |
| `c` · `escape` | Cancel |

### Filename conflict

| Key | Action |
|---|---|
| `o` | Overwrite |
| `s` | Skip |
| `r` | Rename |
| `a` | Don't ask again |
| `c` · `escape` | Cancel |

### File in use

| Key | Action |
|---|---|
| `r` | Retry |
| `s` | Skip |
| `a` | Toggle don't-ask-again |
| `c` · `escape` | Cancel |

### Yes/no

| Key | Action |
|---|---|
| `y` | Yes |
| `n` · `escape` | No |
| `a` | Don't ask again |

### Drag-and-drop paste

| Key | Action |
|---|---|
| `c` | Copy |
| `m` | Move |
| `escape` | Cancel |

Note `c` cancels in the delete/trash/conflict modals but means **copy** here.

---

## Text inputs

| Key | Action |
|---|---|
| `enter` | Submit |
| `ctrl+a` · `home` | Start of line |
| `ctrl+e` · `end` | End of line |
| `left` · `right` | Move by character |
| `ctrl+left` · `ctrl+right` | Move by word |
| `shift+left` · `shift+right` | Extend selection by character |
| `ctrl+shift+left` · `ctrl+shift+right` | Extend selection by word |
| `shift+home` · `shift+end` | Extend to start / end of line |
| `ctrl+shift+a` | Select all |
| `ctrl+w` · `ctrl+backspace` | Delete word left |
| `ctrl+delete` | Delete word right |
| `ctrl+u` | Delete to start of line |
| `ctrl+d` · `delete` | Delete right |
| `ctrl+c` · `super+c` / `ctrl+x` / `ctrl+v` | Copy / cut / paste |

Readline-style bindings, so `ctrl+a` is start-of-line here, not select-all —
select-all is `ctrl+shift+a`.

In the shell screen, `tab` and `shift+tab` cycle mode.

---

## Presets

Three built-in keymaps: `base` (the default above), `vim`, and `sane`. Choose
one in `keys.toml`:

```toml
inherit = "vim"
```

Main differences in the copy menu:

| Action | default | vim | sane |
|---|---|---|---|
| Open copy menu | `Y` | `Y` | `C` |
| Copy to rovr clipboard | `y` | `y` | `r` |
| Copy highlighted path | `p` | `c` | `p` |
| Copy highlighted name 🔧 | `n` | `n` | `n` |
| Copy to system clipboard | `s` | `s` | `s` |
| Copy current directory | `d` | `d` | `u` |

## Rebinding

User keymap lives beside `config.toml`:

| OS | Path |
|---|---|
| Linux | `~/.config/rovr/keys.toml` |
| macOS | `~/Library/Application Support/rovr/keys.toml` |
| Windows | `%LOCALAPPDATA%\rovr\keys.toml` |

Your file merges over the inherited preset, so you only list what changes.
Set a key to `noop` to disable it without rebinding:

```toml
inherit = "base"

[file_list]
"q" = "noop"
"Y" = { action = "copy.name", desc = "Copy the file name" }
```

Printable keys use their character; everything else uses Textual's key names
(`ctrl+j`, `shift+f10`, `pagedown`).
