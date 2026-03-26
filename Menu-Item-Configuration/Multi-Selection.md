# Multi-Selection Behavior

Controls how the menu item behaves when multiple files are selected in Explorer.

## Modes

| Mode | Description |
|------|-------------|
| **Run once for each file** | Executes the command separately for each selected file |
| **Pass all paths at once** | Passes all selected file paths in a single execution |
| **Only show for single selection** | The menu item is hidden when multiple files are selected |

## Pass All Paths at Once

When using this mode:

- Use `[paths]` or `[paths:q]` (quoted) in your launch arguments to receive all selected paths.
- Specify a **delimiter** to separate paths (default is space).
- If you forget to include `[paths]` / `[paths:q]`, only the first file's path will be used.

### Example

**Merge PDFs with a tool:**

```
Target:   C:\Tools\pdfmerge.exe
Args:     --output merged.pdf [paths:q]
```

Selecting `a.pdf`, `b.pdf`, `c.pdf` runs:

```
C:\Tools\pdfmerge.exe --output merged.pdf "a.pdf" "b.pdf" "c.pdf"
```
