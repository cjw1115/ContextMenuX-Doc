# Launch Arguments

Define the executable and arguments for your context menu item.

## Launch Target

The program or script to run. You can **type directly** in the field or use the **Choose** button to pick from the system.

### Input methods

- **Type directly** — enter a command name, path, or protocol URI
- **Choose File** — pick an `.exe`, `.bat` from the file system
- **Browse Installed Apps** — select from installed Windows apps (Win32 & UWP/MSIX)

### Target types

#### Commands on PATH

Programs available in the system `PATH` — no full path needed.

| Target | Description |
|--------|-------------|
| `python` | Run Python scripts |
| `node` | Run Node.js scripts |
| `git` | Git commands |
| `code` | Open with VS Code |
| `notepad` | Open with Notepad |
| `ffmpeg` | Media processing |

#### System commands

Built-in Windows commands and tools.

| Target | Args example | Description |
|--------|-------------|-------------|
| `cmd` | `/c dir` | Run a CMD command |
| `powershell` | `-Command "Get-ChildItem"` | Run a PowerShell command |
| `wt` | `-d [dir]` | Open Windows Terminal |
| `explorer` | `[dir]` | Open folder in Explorer |
| `mstsc` | | Remote Desktop |
| `calc` | | Calculator |

#### Paths with environment variables

Use `%VAR%` syntax for portable paths.

| Target | Description |
|--------|-------------|
| `%USERPROFILE%\scripts\backup.bat` | User-specific script |
| `%APPDATA%\tools\mytool.exe` | Roaming app data tool |
| `%ProgramFiles%\7-Zip\7zFM.exe` | Program Files app |
| `%SystemRoot%\System32\cleanmgr.exe` | System utility |
| `%LOCALAPPDATA%\Programs\oh-my-posh\oh-my-posh.exe` | Local app data tool |

#### Protocol URIs & shell commands

Launch apps via protocol handlers or `shell:` namespace.

| Target | Description |
|--------|-------------|
| `shell:AppsFolder\Microsoft.WindowsTerminal_8wekyb3d8bbwe!App` | Windows Terminal (UWP) |
| `ms-settings:` | Open Windows Settings |
| `ms-settings:display` | Open Display settings |
| `mailto:` | Default mail client |
| `https://github.com` | Open URL in default browser |

## Arguments

Optional command-line arguments passed to the target. A live preview of the final command is shown below the input.

- If the arguments field is **empty** and no path variables are used, the clicked file path is automatically passed as the argument.
- If the arguments field contains **custom text without any path variables**, the arguments are used as-is — the clicked file path is **not** appended automatically.

## Run as Administrator

Enable the **Run as Administrator** checkbox to launch the target with elevated privileges. This triggers a UAC prompt when the menu item is clicked.

This uses `ShellExecute` with the `"runas"` verb internally — no PowerShell workaround needed.

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
| `[folder]` | The folder itself (folder) or parent folder (file) | see below |
| `[folder:q]` | Same as `[folder]`, quoted | see below |
| `[paths]` | All selected paths (multi-select) | `a.txt b.txt` |
| `[paths:q]` | All selected paths, quoted | `"a.txt" "b.txt"` |

> **Tip:** Use quoted variants (`[path:q]`, `[dir:q]`, `[folder:q]`) when paths may contain spaces.

### `[folder]` vs `[dir]`

`[dir]` always resolves to the **parent** of the right-clicked item. This works well for files, but can be confusing for folders:

| Variable | Right-click file `C:\Docs\report.pdf` | Right-click folder `C:\Docs\Photos` |
|----------|--------------------------------------|--------------------------------------|
| `[dir]` | `C:\Docs` | `C:\Docs` (parent of Photos) |
| `[folder]` | `C:\Docs` | `C:\Docs\Photos` (the folder itself) |

Use `[folder]` when you want "the folder" regardless of whether the user clicked a file or a folder.

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
