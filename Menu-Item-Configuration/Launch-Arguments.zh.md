# 启动参数

定义右键菜单项的可执行程序和参数。

## 启动目标

要运行的程序或脚本。你可以在输入框中**直接输入**，也可以使用**选择**按钮从系统中选取。

### 输入方式

- **直接输入** — 输入命令名称、路径或协议 URI
- **选择文件** — 从文件系统中选取 `.exe`、`.bat` 文件
- **浏览已安装应用** — 从已安装的 Windows 应用中选择（Win32 和 UWP/MSIX）

### 目标类型

#### PATH 中的命令

系统 `PATH` 中可用的程序——无需完整路径。

| 目标 | 说明 |
|------|------|
| `python` | 运行 Python 脚本 |
| `node` | 运行 Node.js 脚本 |
| `git` | Git 命令 |
| `code` | 用 VS Code 打开 |
| `notepad` | 用记事本打开 |
| `ffmpeg` | 媒体处理 |

#### 系统命令

Windows 内置命令和工具。

| 目标 | 参数示例 | 说明 |
|------|----------|------|
| `cmd` | `/c dir` | 运行 CMD 命令 |
| `powershell` | `-Command "Get-ChildItem"` | 运行 PowerShell 命令 |
| `wt` | `-d [dir]` | 打开 Windows Terminal |
| `explorer` | `[dir]` | 在资源管理器中打开文件夹 |
| `mstsc` | | 远程桌面 |
| `calc` | | 计算器 |

#### 包含环境变量的路径

使用 `%VAR%` 语法实现可移植路径。

| 目标 | 说明 |
|------|------|
| `%USERPROFILE%\scripts\backup.bat` | 用户专属脚本 |
| `%APPDATA%\tools\mytool.exe` | 漫游应用数据工具 |
| `%ProgramFiles%\7-Zip\7zFM.exe` | Program Files 中的应用 |
| `%SystemRoot%\System32\cleanmgr.exe` | 系统工具 |
| `%LOCALAPPDATA%\Programs\oh-my-posh\oh-my-posh.exe` | 本地应用数据工具 |

#### 协议 URI 和 shell 命令

通过协议处理程序或 `shell:` 命名空间启动应用。

| 目标 | 说明 |
|------|------|
| `shell:AppsFolder\Microsoft.WindowsTerminal_8wekyb3d8bbwe!App` | Windows Terminal (UWP) |
| `ms-settings:` | 打开 Windows 设置 |
| `ms-settings:display` | 打开显示设置 |
| `mailto:` | 默认邮件客户端 |
| `https://github.com` | 在默认浏览器中打开 URL |

## 参数

传递给目标的可选命令行参数。输入框下方会显示最终命令的实时预览。

- 如果参数字段为**空**且未使用路径变量，则右键点击的文件路径会自动作为参数传递。
- 如果参数字段包含**没有路径变量的自定义文本**，则按原样使用参数——右键点击的文件路径**不会**自动追加。
- 若要**不传递任何参数**启动，请将参数设为 `[empty]`。这会阻止自动传递文件路径。

## 以管理员身份运行

勾选**以管理员身份运行**复选框，以提升权限启动目标程序。点击菜单项时会触发 UAC 提示。

内部使用 `ShellExecute` 的 `"runas"` 动词——无需 PowerShell 变通方案。

## 路径变量

插入在运行时展开为右键点击文件属性的变量：

| 变量 | 说明 | 示例值 |
|------|------|--------|
| `[path]` | 完整文件路径 | `C:\Docs\report.pdf` |
| `[path:q]` | 完整文件路径（带引号） | `"C:\Docs\report.pdf"` |
| `[name]` | 带扩展名的文件名 | `report.pdf` |
| `[stem]` | 不带扩展名的文件名 | `report` |
| `[ext]` | 文件扩展名 | `.pdf` |
| `[dirname]` | 父文件夹名称 | `Docs` |
| `[dir]` | 父文件夹路径 | `C:\Docs` |
| `[dir:q]` | 父文件夹路径（带引号） | `"C:\Docs"` |
| `[folder]` | 文件夹本身（文件夹）或父文件夹（文件） | 见下文 |
| `[folder:q]` | 同 `[folder]`（带引号） | 见下文 |
| `[paths]` | 所有选中的路径（多选） | `a.txt b.txt` |
| `[paths:q]` | 所有选中的路径（带引号） | `"a.txt" "b.txt"` |
| `[empty]` | 明确不传递任何参数 | *（空）* |

> **提示：** 当路径可能包含空格时，请使用带引号的变体（`[path:q]`、`[dir:q]`、`[folder:q]`）。

### `[folder]` 与 `[dir]` 的区别

`[dir]` 始终解析为右键点击项目的**父目录**。这对文件来说没问题，但对文件夹可能会造成困惑：

| 变量 | 右键点击文件 `C:\Docs\report.pdf` | 右键点击文件夹 `C:\Docs\Photos` |
|------|--------------------------------------|--------------------------------------|
| `[dir]` | `C:\Docs` | `C:\Docs`（Photos 的父目录） |
| `[folder]` | `C:\Docs` | `C:\Docs\Photos`（文件夹本身） |

当你需要"目标文件夹"而不管用户点击的是文件还是文件夹时，请使用 `[folder]`。

## 示例

> 每个示例标注了所使用的变量，方便你快速查找参考。
> 完整的可导入配置请参阅 [examples/menus_config.json](../examples/menus_config.json)。

### 用 VS Code 打开 — `[path:q]`

```
Target:   C:\Program Files\Microsoft VS Code\Code.exe
Args:     [path:q]
```

### 在 Windows Terminal 中打开文件夹（UWP 应用）— `[path:q]`

```
Target:   Microsoft.WindowsTerminal_8wekyb3d8bbwe!App
Args:     -d [path:q]
```

> **提示：** 使用**浏览已安装应用**来选择 UWP/MSIX 应用（如 Windows Terminal）。

### 复制文件路径到剪贴板 — `[path]`

```
Target:   cmd.exe
Args:     /c echo [path]| clip
```

### 重命名文件：以父文件夹名为前缀 — `[path:q]` `[dirname]` `[name]`

```
Target:   cmd.exe
Args:     /c ren [path:q] "[dirname]_[name]"
```

右键点击 `C:\Photos\Trip\sunset.jpg` → 重命名为 `Trip_sunset.jpg`

### FFmpeg 转换视频为 MP4 — `[path:q]` `[dir]` `[stem]`

```
Target:   ffmpeg.exe
Args:     -i [path:q] -c:v libx264 -c:a aac "[dir]\[stem]_converted.mp4"
```

### 7-Zip 解压到子文件夹 — `[path:q]` `[dir:q]` `[stem]`

```
Target:   C:\Program Files\7-Zip\7z.exe
Args:     x [path:q] -o[dir:q]\[stem]
```

### 用 Python 运行点击的文件 — `[path:q]`

```
Target:   python.exe
Args:     [path:q]
```

右键点击 `.py` 文件 → 直接用 Python 执行。
