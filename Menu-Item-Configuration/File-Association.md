# File Association

Control which file types and locations trigger your context menu item.

## Quick Options

| Option | Effect |
|--------|--------|
| **All Files** | Matches any file (`*`) |
| **Folder** | Appears on folder right-click |
| **Desktop** | Appears on desktop background right-click |

## Custom File Extensions

Add specific extensions (e.g. `.png`, `.txt`, `.log`) to limit the menu item to those file types.

- Type an extension starting with `.` and press Enter to add it.
- Use **Detect from File** to auto-detect the extension from an existing file.
- Click the `×` on a tag to remove it.

## Validation

Extensions must:
- Start with a period (`.`)
- Not contain `\ / : * ? " < > |`
- Not end with a space or period

## Example

To create a menu item that only appears for image files:

Add: `.png`, `.jpg`, `.jpeg`, `.bmp`, `.gif`, `.webp`
