# Launch Arguments

Define the executable and arguments for your context menu item.

## Launch Target

The program or script to run. Choose from:
- **Choose File** — pick an `.exe`, `.bat`.
- **Browse Installed Apps** — select from installed Windows apps

## Arguments

Optional command-line arguments passed to the target. A live preview of the final command is shown below the input.

## Path Variables

Insert variables that expand to properties of the right-clicked file at runtime:

| Variable | Description | Example Value |
|----------|-------------|---------------|
| `[path]` | Full file path | `C:\Docs\report.pdf` |
| `[path:q]` | Full file path, quoted | `"C:\Docs\report.pdf"` |
| `[name]` | File name with extension | `report.pdf` |
| `[stem]` | File name without extension | `report` |
| `[ext]` | File extension | `.pdf` |
| `[dirname]` | Parent folder name | `Docs` |
| `[dir]` | Parent folder path | `C:\Docs` |
| `[dir:q]` | Parent folder path, quoted | `"C:\Docs"` |
| `[paths]` | All selected paths (multi-select) | `a.txt b.txt` |
| `[paths:q]` | All selected paths, quoted | `"a.txt" "b.txt"` |

> **Tip:** Use quoted variants (`[path:q]`, `[dir:q]`) when paths may contain spaces.

## Examples

> Each example marks the variables it uses, so you can quickly find a reference for any variable.
> A complete importable config is available at [examples/menus_config.json](../examples/menus_config.json).

### Open in VS Code — `[path:q]`

```
Target:   C:\Program Files\Microsoft VS Code\Code.exe
Args:     [path:q]
```

### Open folder in Windows Terminal (UWP App) — `[path:q]`

```
Target:   Microsoft.WindowsTerminal_8wekyb3d8bbwe!App
Args:     -d [path:q]
```

> **Tip:** Use **Browse Installed Apps** to select UWP/MSIX apps like Windows Terminal.

### Copy file path to clipboard — `[path]`

```
Target:   cmd.exe
Args:     /c echo [path]| clip
```

### Rename file: add prefix with parent folder name — `[path:q]` `[dirname]` `[name]`

```
Target:   cmd.exe
Args:     /c ren [path:q] "[dirname]_[name]"
```

Right-click `C:\Photos\Trip\sunset.jpg` → renames to `Trip_sunset.jpg`

### FFmpeg convert video to MP4 — `[path:q]` `[dir]` `[stem]`

```
Target:   ffmpeg.exe
Args:     -i [path:q] -c:v libx264 -c:a aac "[dir]\[stem]_converted.mp4"
```

### 7-Zip extract to subfolder — `[path:q]` `[dir:q]` `[stem]`

```
Target:   C:\Program Files\7-Zip\7z.exe
Args:     x [path:q] -o[dir:q]\[stem]
```

### Run Python script on clicked file — `[path:q]`

```
Target:   python.exe
Args:     [path:q]
```

Right-click a `.py` file → execute it directly with Python.
