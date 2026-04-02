# CLI Getting Started

This guide covers Xplorer's CLI usage. For the interactive TUI, see the [Getting Started](getting-started.md) guide.

## Prerequisites

- Kubernetes cluster with Crossplane installed
- `kubectl` configured with cluster access
- At least one Crossplane claim in your cluster

## Installation

### macOS / Linux (Homebrew)

```bash
brew tap xplorerhq/dist
brew install xplorer
```

### Direct Download

Download the binary for your platform from [Releases](https://github.com/XplorerHQ/xplorer-community/releases):

| Platform | Asset |
|----------|-------|
| macOS (Apple Silicon) | `xplorer-<version>-darwin-arm64.tar.gz` |
| macOS (Intel) | `xplorer-<version>-darwin-x64.tar.gz` |
| Linux (x64) | `xplorer-<version>-linux-x64.tar.gz` |
| Windows (x64) | `xplorer-<version>-windows-x64.zip` |

**Linux / macOS:**
```bash
tar -xzf xplorer-<version>-<platform>.tar.gz
sudo mv xplorer/xplorer /usr/local/bin/
```

**Windows:**
Extract the zip and add the `xplorer` directory to your `PATH`.

### Verify Installation

```bash
xplorer --version
```

## Your First Exploration

### 1. Launch the TUI

The fastest way to explore your cluster:

```bash
xplorer tui
```

The TUI provides a tree view of all resources with keyboard navigation, syntax-highlighted manifests, live updates, and the ability to pause/resume reconciliation. See [TUI Quick Start](getting-started.md) for a detailed walkthrough.

### 2. List Available Claims (CLI)

See all Crossplane claims in your cluster:

```bash
xplorer list --claims
```

This shows claims across all namespaces with their health status.

### 3. Explore a Claim

Pick a claim and explore its resource hierarchy:

```bash
xplorer show <claim-name>
```

This displays the claim and its immediate composite resource (XR).

### 4. See the Full Hierarchy

Add `-v` to see managed resources:

```bash
xplorer show <claim-name> -v
```

This shows:
- The claim
- The composite resource (XR)
- All managed resources with health status
- Error details for unhealthy resources

### 5. Interactive Mode

Running `xplorer` with no arguments launches an interactive resource selector powered by [fzf](https://github.com/junegunn/fzf). It lets you fuzzy-search through all claims and XRs, then runs the appropriate CLI command for the selected resource.

```bash
xplorer
```

![Interactive Mode](images/legacy-tui.png)

This is a lightweight alternative when you want quick CLI output without launching the full TUI. If `fzf` is not installed, Xplorer falls back to a simpler built-in selector with limited functionality. For the best experience, install `fzf` via your package manager (`brew install fzf` on macOS).

## Understanding the Output

### Health Status

- **Ready: True** - Resource is fully operational
- **Ready: False** - Resource has issues (check error details)
- **Synced: True** - Resource matches desired state
- **Synced: False** - Resource is out of sync

### Resource Tree

```
my-database (Claim)
└── my-database-xyz (XR: XDatabase)
    ├── my-database-xyz-abc (RDSInstance) - Ready
    ├── my-database-xyz-def (SecurityGroup) - Ready
    └── my-database-xyz-ghi (SubnetGroup) - Ready: False ← Issue here
```

## Next Steps

- See [TUI Quick Start](getting-started.md) for the interactive terminal UI
- See [Keyboard Shortcuts](keyboard-shortcuts.md) for the full shortcut reference
- See [CLI Reference](cli-reference.md) for all commands
- Use `xplorer show <claim> -v` for full hierarchy with all managed resources
- Try `xplorer watch <claim>` to monitor changes in real-time
