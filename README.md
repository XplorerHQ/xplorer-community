# Xplorer

**Crossplane resource explorer** - Trace claims through composite resources (XRs) to managed resources.

<!-- TODO: Add demo GIF here -->
<!-- ![Xplorer Demo](docs/assets/demo.gif) -->

## Why Xplorer?

When working with Crossplane, understanding resource relationships and debugging issues is challenging. With `kubectl` you can inspect individual resources, but tracing through the hierarchy requires many commands:

```bash
kubectl get claims -A
kubectl get composite
kubectl get managed
kubectl describe <claim>
kubectl describe <xr>
kubectl describe <managed-resource>
kubectl get events --field-selector involvedObject.name=<resource>
# ...and manually correlating all of this
```

**Xplorer solves this by:**

- Automatically discovering the complete resource hierarchy from a claim
- Showing health status propagation through the tree
- Highlighting problematic resources with error details and events
- Providing a single command to understand what's happening

What would take multiple `kubectl` commands and manual piecing together becomes:

```bash
xplorer show my-claim -v
```

## Installation

### macOS / Linux (Homebrew)

```bash
brew tap xplorerhq/dist
brew install xplorer
```

### Windows (Scoop)

```powershell
scoop bucket add xplorer https://github.com/XplorerHQ/scoop-xplorer
scoop install xplorer
```

### Direct Download

Download binaries from [Releases](https://github.com/XplorerHQ/homebrew-dist/releases).

## Quick Start

```bash
# List all Crossplane claims
xplorer list --claims

# Explore a specific claim's hierarchy
xplorer show my-database-claim

# Show full hierarchy with health status
xplorer show my-database-claim -v

# Watch a claim for changes
xplorer watch my-database-claim
```

## Features

- **Hierarchy Discovery** - Automatically traces Claims → XRs → Managed Resources
- **Health Status** - Shows Ready/Synced status with propagation through the tree
- **Error Details** - Surfaces error messages and related events
- **Interactive Selection** - Fuzzy search when no claim specified
- **TUI Mode** - Full terminal UI for exploration (`xplorer-tui`)
- **Watch Mode** - Real-time updates as resources change

## Documentation

- [Getting Started](docs/getting-started.md)
- [CLI Reference](docs/cli-reference.md)
- [FAQ](docs/faq.md)

## Requirements

- Kubernetes cluster with Crossplane installed
- `kubectl` configured with cluster access

## Status

**Alpha** - Actively developed. Feedback welcome. [Report issues](https://github.com/XplorerHQ/xplorer-community/issues).

## Feedback & Support

- [Report a Bug](https://github.com/XplorerHQ/xplorer-community/issues/new?template=bug_report.md)
- [Request a Feature](https://github.com/XplorerHQ/xplorer-community/issues/new?template=feature_request.md)
- [Ask a Question](https://github.com/XplorerHQ/xplorer-community/discussions)

## Sponsor

If Xplorer helps your workflow, consider [sponsoring](https://github.com/sponsors/XplorerHQ) to support development.

---

© 2025 Alex Hubitski. All rights reserved. Free for evaluation.
