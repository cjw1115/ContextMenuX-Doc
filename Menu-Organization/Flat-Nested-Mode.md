# Flat / Nested Mode

Controls how your menu items are organized in the Windows 11 modern context menu.

## Flat Mode

Each menu item appears as a **separate top-level entry** in the context menu.

```
Right-click menu:
├── Open with Tool A
├── Convert to PDF
└── Upload to Server
```

Best for: a small number of items that you want quick access to.

## Nested Mode

All menu items are grouped under a **single parent entry** with a custom name.

```
Right-click menu:
└── My Tools ▸
    ├── Open with Tool A
    ├── Convert to PDF
    └── Upload to Server
```

- Enter a **display name** for the parent entry (e.g. "My Tools").
- **Only works in the Windows 11 modern context menu.** Nested mode leverages the system's automatic folding capability, which is not available in the classic menu.

Best for: keeping the context menu clean when you have many items.

## Submenu Containers

In either mode, you can create hierarchical structures by toggling **"Has child items"** on a menu item. This turns it into a submenu container that holds child items instead of running a command.
