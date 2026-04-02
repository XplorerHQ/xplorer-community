# FAQ

Frequently asked questions about Xplorer.

## General

### What is Xplorer?

Xplorer is a CLI and TUI tool for exploring Crossplane resources. It automatically traces the hierarchy from claims through composite resources (XRs) to managed resources, showing health status and errors along the way.

### What problem does it solve?

When debugging Crossplane, you typically need to run multiple `kubectl` commands to trace from a claim to its XR to the managed resources. Xplorer does this automatically -- either through the interactive TUI or a single CLI command.

### Is Xplorer open source?

Xplorer is currently closed source. During the alpha phase, all features are fully enabled and free to use. As the project matures, we plan to offer a generous community edition covering the core workflows, with an enterprise tier for advanced capabilities. The exact feature split is still being shaped -- your feedback during the alpha helps inform what belongs where.

## Installation

### Which platforms are supported?

- macOS (Intel and Apple Silicon)
- Linux (x64)
- Windows (x64)

### How do I update Xplorer?

**Homebrew (macOS/Linux):**
```bash
brew upgrade xplorer
```

**Direct download:**
Download the latest binary from [Releases](https://github.com/XplorerHQ/xplorer-community/releases) and replace the existing binary.

## TUI

### How do I launch the TUI?

```bash
xplorer tui
```

Or use the direct alias `xplorer-tui`. See [Getting Started](getting-started.md) for a full walkthrough.

### Where can I find all keyboard shortcuts?

Press `F1` on any screen for contextual help, or see the full [Keyboard Shortcuts](keyboard-shortcuts.md) reference.

### The TUI is slow to start on my cluster

The initial load discovers all Crossplane resources in the cluster. Xplorer's discovery is heavily optimized and significantly faster than equivalent `kubectl` operations, but on very large clusters it can still take a moment. Once loaded, navigation is instant and live updates arrive via the Kubernetes watch stream without re-scanning.

If startup time is still a concern, you can bypass discovery entirely by using the CLI with known resource names (e.g., `xplorer show my-claim -v`). For improving TUI performance on your specific cluster, please [open an issue](https://github.com/XplorerHQ/xplorer-community/issues) with details about your cluster size and resource count -- real-world performance data helps us tune caching and freshness guarantees across a broader set of environments.

### Can I pause resources from the TUI?

Yes. Navigate to any resource and press `p` to toggle pause, or `Shift+P` to cascade the pause across the resource and all its children. This adds or removes the `crossplane.io/paused` annotation. A confirmation dialog is shown before any pause operation.

### Alt key shortcuts don't work in iTerm2

By default, iTerm2 on macOS treats the Option key as a compose key. Go to **Preferences > Profiles > Keys** and set **Left Option Key** to **Esc+**. See the [iTerm2 configuration section](getting-started.md#iterm2-alt-key-configuration) in the getting started guide.

### How does the unhealthy scan work?

Press `Ctrl+U` to scan all compositions. Xplorer detects two cases: resources where the top-level status is not True, and resources where the parent reports healthy but underlying managed resources are failing. The scan traverses the full hierarchy of every composition, so it can take a moment on large clusters.

### Can I change the color theme?

Yes. Press `Alt+T` to cycle through themes. The same shortcut switches the YAML syntax theme when the manifest panel is open. Your selection is saved to `~/.config/xplorer/config.yaml` and persists across restarts.

### What is the Universal Resource Palette?

A work-in-progress feature (`Ctrl+X, R`) that lets you browse any Kubernetes resource in the cluster, not just Crossplane resources. It handles the kind collision problem in Crossplane v2 where the same kind exists in both legacy and `.m.` API groups.

## CLI

### Why is the CLI tree output so compact?

By default, the CLI renders a minimal tree when a claim is healthy -- just the claim and its XR -- to save terminal space. Unhealthy resources and their error details are always shown. If you want the full tree including all healthy managed resources, use `-v`.

### Why can't Xplorer find my claims?

Common causes:
1. **Wrong namespace** - Use `-n <namespace>` or check all namespaces
2. **Missing permissions** - Ensure your kubeconfig has access to claim resources
3. **No claims exist** - Verify claims exist with `kubectl get claims`

### What does "Ready: False" mean?

The resource hasn't reached a healthy state. Use `-v` flag to see error details and related events, or launch the TUI for an interactive view.

### What does "Synced: False" mean?

The resource's actual state doesn't match its desired state. Crossplane is either still reconciling or encountered an error.

### How do I see more details?

Use the `-v` flag to show all managed resources in the hierarchy, including healthy ones.

### Can I use Xplorer with multiple clusters?

Yes, use the `--context` flag:
```bash
xplorer --context production show my-claim
```

The TUI also supports this: `xplorer tui --context production`.

### What is the interactive fzf mode?

Running `xplorer` with no arguments launches an interactive resource selector powered by [fzf](https://github.com/junegunn/fzf). If fzf is not installed, a simpler fallback selector is used with limited functionality.

## Troubleshooting

### Xplorer is slow on large clusters

Try:
1. Use the TUI -- it loads once and then uses live updates via watch stream
2. Filter by namespace: `xplorer list -n specific-namespace`
3. Target specific claims: `xplorer show specific-claim`

### I see permission errors

Ensure your kubeconfig has permissions to:
- List and get Claims
- List and get Composite Resources (XRs)
- List and get Managed Resources
- List Events (for error details)

### Debug logging

Enable debug mode for detailed logs:
```bash
xplorer --debug show my-claim
```

Logs are saved to `~/.xplorer/logs/`

## Getting Help

- [Report a bug](https://github.com/XplorerHQ/xplorer-community/issues/new?template=bug_report.md)
- [Request a feature](https://github.com/XplorerHQ/xplorer-community/issues/new?template=feature_request.md)
- [Ask a question](https://github.com/XplorerHQ/xplorer-community/discussions)
