# Window Style

Control how the launched application window appears.

## Options

| Style | Description |
|-------|-------------|
| **Normal** | Window appears at its default size and position (default) |
| **Minimized** | Window starts minimized to the taskbar |
| **Maximized** | Window starts maximized to fill the screen |
| **Hidden** | Window is launched invisibly — useful for background scripts or silent commands |

## JSON Configuration

```json
{
    "WindowStyle": "Hidden"
}
```

Accepts: `Normal`, `Minimized`, `Maximized`, `Hidden` (case-insensitive). If omitted, defaults to `Normal`.
