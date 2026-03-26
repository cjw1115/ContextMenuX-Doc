# Display Conditions

Display conditions let you control when a context menu item appears, based on the properties of the right-clicked file or folder.

## Condition Fields

Each condition matches against one of these file properties:

| Field | Variable | Description | Example |
|-------|----------|-------------|---------|
| Path | `[path]` | Full path of the clicked item | `C:\Users\me\Documents\report.pdf` |
| Name | `[name]` | File name including extension | `report.pdf` |
| Name Without Extension | `[stem]` | File name without extension | `report` |
| Extension | `[ext]` | File extension (with dot) | `.pdf` |
| Parent Folder Name | `[dirname]` | Name of the parent directory | `Documents` |
| Parent Folder | `[dir]` | Full path of the parent directory | `C:\Users\me\Documents` |

## Match Modes

| Mode | Description | Example Pattern | Matches |
|------|-------------|-----------------|---------|
| Exact | Case-insensitive exact match | `.pdf` | `.pdf` only |
| Contains | Value contains the pattern | `report` | `report.pdf`, `annual_report.docx` |
| Starts With | Value starts with the pattern | `IMG_` | `IMG_001.jpg`, `IMG_photo.png` |
| Ends With | Value ends with the pattern | `_backup` | `data_backup`, `db_backup` |
| Wildcard | Glob-style `*` and `?` matching | `*.log` | `app.log`, `error.log` |
| Regex | Regular expression (case-insensitive) | `^IMG_\d+$` | `IMG_001`, `IMG_9999` |

## Behavior: Show if / Hide if

Each condition is either a **Show if** rule or a **Hide if** rule.

- **Show if** — the menu item should appear when this condition matches.
- **Hide if** — the menu item should be hidden when this condition matches.

## How Multiple Conditions Combine

When multiple conditions are defined, they combine using the following logic:

1. **Hide if** rules are evaluated first with **OR** — if **any** Hide rule matches, the item is hidden immediately.
2. **Show if** rules are evaluated with **OR** — if **any** Show rule matches, the item passes the show check.
3. If Show rules exist but **none** match, the item is hidden.
4. If no Show rules exist, the item is shown by default (only Hide rules apply).

In short: **Show rules OR together, Hide rules OR together, Hide wins over Show.**

### Examples

#### Show for multiple file types

| Rule | |
|------|-|
| Show if Extension equals `.jpg` | |
| Show if Extension equals `.png` | |
| Show if Extension equals `.gif` | |

Result: the menu item appears for any `.jpg`, `.png`, or `.gif` file.

#### Show for a type but exclude certain names

| Rule | |
|------|-|
| Show if Extension equals `.log` | |
| Hide if Name contains `debug` | |

| File | Result |
|------|--------|
| `app.log` | Shown |
| `debug.log` | Hidden (Hide rule matches) |
| `report.pdf` | Hidden (no Show rule matches) |

#### Hide-only rules (no Show rules)

| Rule | |
|------|-|
| Hide if Name contains `temp` | |
| Hide if Extension equals `.bak` | |

Result: the menu item appears for everything **except** files with `temp` in the name or `.bak` extension.
