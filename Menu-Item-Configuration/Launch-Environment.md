# Working Directory

Set the initial working directory for the launched process.

**Default:** `[dir]` — the parent folder of the right-clicked file.

## Available Variables

| Variable | Description | Example Value |
|----------|-------------|---------------|
| `[folder]` | The folder itself (folder) or parent folder (file) | `C:\Docs` |
| `[dir]` | Parent folder path | `C:\Docs` |
| `[dirname]` | Parent folder name | `Docs` |
| `[stem]` | File name without extension | `report` |
| `[name]` | File name with extension | `report.pdf` |

> **Tip:** Use `[folder]` instead of `[dir]` if your menu item targets both files and folders — it always resolves to the relevant folder. See [Launch Arguments → `[folder]` vs `[dir]`](Launch-Arguments.md#folder-vs-dir) for details.

> **Note:** Quoted variants (`[dir:q]`, `[path:q]`, `[folder:q]`) and file-extension variables (`[ext]`) are not available here because the working directory must be a raw, unquoted path — `ShellExecute` does not accept quoted paths in the `lpDirectory` parameter.

> **Note:** When the working directory field is left empty, it defaults to the parent folder of the clicked file (`[dir]`).

## JSON Configuration

```json
{
    "WorkingDirectory": "[dir]"
}
```

Accepts any string with path variables. If omitted or empty, defaults to the parent folder of the clicked file.
