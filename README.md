# Notilus

A Finder-style desktop file manager built in Flutter, with a built-in Ollama
chat panel and a workflow editor for running custom prompt chains against your
local files.

## Features

- **File browser** — sidebar with favorites, breadcrumb path bar, list view,
  and an info panel for the selected item. Cupertino UI with light/dark themes.
- **Ollama chat panel** — talk to any model you have pulled locally. Streams
  responses token-by-token via `/api/generate`.
- **Workflow editor** — build multi-step prompt chains (each step has its own
  template and optional model override) and run them as repeatable workflows.
- **Local settings** — preferences and saved workflows persist via
  `shared_preferences`; no cloud, no account.

## Install

Prebuilt binaries are published on the
[Releases page](https://github.com/JayashBhandary/Notilus/releases).

### One-liner

**macOS / Linux:**

```sh
curl -fsSL https://raw.githubusercontent.com/JayashBhandary/Notilus/main/install.sh | sh
```

**Windows (PowerShell):**

```powershell
irm https://raw.githubusercontent.com/JayashBhandary/Notilus/main/install.ps1 | iex
```

The installer detects your platform, downloads the latest release asset, and
installs to:

| Platform | Install location |
|---|---|
| macOS | `/Applications/Notilus.app` |
| Linux | `/opt/notilus` (symlinked at `/usr/local/bin/notilus`) |
| Windows | `%LOCALAPPDATA%\Programs\Notilus` (Start Menu shortcut added) |

### Manual download

Grab the asset for your platform from the
[Releases page](https://github.com/JayashBhandary/Notilus/releases/latest):

| Asset | Target |
|---|---|
| `Notilus-<v>-macos-arm64.dmg` | Apple Silicon Macs (M1/M2/M3+) |
| `Notilus-<v>-macos-x64.dmg` | Intel Macs |
| `Notilus-<v>-macos-universal.dmg` | Either architecture (larger file) |
| `Notilus-<v>-windows-x64.zip` | Windows 10/11 x64 |
| `Notilus-<v>-linux-x64.tar.gz` | Linux x86_64 |

> macOS builds are ad-hoc signed but **not notarized**. The install script
> strips the Gatekeeper quarantine attribute for you. If you download a DMG
> directly through a browser and macOS refuses to open it, run:
> `xattr -dr com.apple.quarantine /Applications/Notilus.app`

## Project layout

```
lib/
├── app.dart                 # MultiProvider + CupertinoApp wiring
├── main.dart
├── theme.dart
├── models/                  # FileEntry, ChatMessage, Workflow, WorkflowStep
├── providers/               # Browser, Chat, Settings, Workflow state
├── screens/                 # Home, Settings, SystemOverview, WorkflowEditor
├── services/                # FileService, OllamaService, SettingsStore, ...
└── widgets/                 # Sidebar, FileListView, ChatPanel, ...
```

## Ollama setup

Notilus talks to a local [Ollama](https://ollama.com) instance (default host
`http://localhost:11434`). Pull at least one model before launching:

```sh
ollama pull llama3.2
```

Point the app at a different Ollama host from **Settings** if needed, then
pick a model and start chatting or create a workflow.

## Build from source

### Prerequisites

- Flutter `>=3.10.0` with Dart `>=3.0.0`
- A running Ollama instance (see above)

### Run

```sh
flutter pub get
flutter run -d macos      # or: linux, windows
```

### Build release binaries

```sh
flutter build macos       # produces a universal .app
flutter build windows
flutter build linux
```

Releases are produced by [`.github/workflows/release.yml`](.github/workflows/release.yml):
push a tag like `v0.1.1` (or run the workflow manually) and CI builds all
five artifacts and attaches them to a GitHub Release.

## Workflows

A workflow is an ordered list of `WorkflowStep`s. Each step has:

- `name` — display label
- `prompt` — prompt template for that step
- `model` *(optional)* — overrides the default chat model for this step only

Workflows are saved as JSON via `SettingsStore` and can be re-run from the
workflow tab on the home screen.

## Status

Version `0.1.1+2` — early, single-developer project. Desktop targets
(macOS / Linux / Windows) are the focus; mobile targets are scaffolded but
not the primary use case.
