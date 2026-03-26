# Working Directory

Set the initial working directory for the launched process.

**Default:** `[dir]` — the parent folder of the right-clicked file.

## Available Variables

| Variable | Description | Example Value |
|----------|-------------|---------------|
| `[dir]` | Parent folder path | `C:\Docs` |
| `[dirname]` | Parent folder name | `Docs` |
| `[stem]` | File name without extension | `report` |
| `[name]` | File name with extension | `report.pdf` |

> **Note:** Quoted variants (`[dir:q]`, `[path:q]`) and file-extension variables (`[ext]`) are not available here because the working directory must be a raw, unquoted path — `ShellExecute` does not accept quoted paths in the `lpDirectory` parameter.

> **Note:** When the working directory field is left empty, it defaults to the parent folder of the clicked file (`[dir]`).

## JSON Configuration

```json
{
    "WorkingDirectory": "[dir]"
}
```

Accepts any string with path variables. If omitted or empty, defaults to the parent folder of the clicked file.
