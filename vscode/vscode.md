# VS Code tips & tricks

---

## launch, tasks, and workspaces json file settings

| File              | Purpose                                                             |
| ----------------- | ------------------------------------------------------------------- |
| launch.json       | Defines how to run or debug your app (e.g., executable, args, env). |
| tasks.json        | Defines commands like build, test, lint, or custom scripts.         |
| `.code-workspace` | Stores workspace-wide settings, folder structure, and extensions.   |

### 🚀 launch.json — Debug Configurations

> Located in .vscode/launch.json

Controls what happens when you press F5 or click Run

Key fields:

- "type": debugger type (e.g., cppdbg, node, coreclr)

- "request": launch or attach

- "program": path to the executable

- "args": command-line arguments

- "preLaunchTask": links to a task in tasks.json

Example:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch MyApp",
      "type": "cppdbg",
      "request": "launch",
      "program": "${workspaceFolder}/build/myapp",
      "args": [],
      "preLaunchTask": "build-myapp"
    }
  ]
}
```

### 🛠️ tasks.json — Build & Automation

- Located in `.vscode/tasks.json`

- Defines reusable commands (build, test, format, etc.)

- Can be triggered manually or linked to launch.json

Example:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build-myapp",
      "type": "shell",
      "command": "make",
      "args": ["myapp"],
      "group": "build"
    }
  ]
}
```

### 🗂️ .code-workspace — Workspace Settings

- Define which folders are part of the workspace (multi-root or single-root)

- Stores:

  - Folder list

  - Settings (settings, extensions, launch, etc.)

Can be saved via File → Save Workspace As…

Example:

```json
{
  "folders": [
    { "path": "projectA" },
    { "path": "projectB" }
  ],
  "settings": {
    "editor.tabSize": 4
  }
}
```
