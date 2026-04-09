# Getting Started

Xplorer's Terminal UI (TUI) is the fastest way to understand what's happening inside a Crossplane cluster. This guide walks through the TUI. For CLI usage, see the [CLI Getting Started](cli-getting-started.md) guide.

## Launching the TUI

---

```bash
xplorer tui
```

## Overview

---

When the TUI launches it connects to your cluster, discovers all Crossplane resources, and presents them in a searchable list.

![TUI Overview](images/tui-overview.svg)

The header shows your cluster connection — context, cluster name, API server — with navigation breadcrumbs. The resource list sits in the middle, and the footer adapts its keyboard shortcuts to whatever panel you're in. Press `F2` to collapse the header and give the list more room; breadcrumbs stay in a single line so you keep navigation context without the overhead.

![No Header](images/tui-overview-no-header.svg)

Each row shows Ready (`R`) and Synced (`S`) status at a glance, alongside resource type, name, kind, namespace, and age. Namespace and Age appear on wider terminals.

<table><tr><td valign="top">

| Column | Meaning |
|--------|---------|
| `R` | Ready (`✓` = True, blank = False) |
| `S` | Synced (`✓` = True, blank = False) |
| `T` | Type (`c` = Claim, `x` = XR) |
| `Name` | Resource name |

</td><td valign="top">

| Column | Meaning |
|--------|---------|
| `Kind` | Crossplane resource kind |
| `NS` | Namespace (wider terminals) |
| `Age` | Time since creation (wider terminals) |

</td></tr></table>

### Searching

---

Start typing to filter with fuzzy matching. Use more than three characters for best results, and space-separate terms to narrow down quickly — `hub backup` matches resources containing both "hub" and "backup" anywhere in name, kind, or namespace.

![Filtering — narrow match](images/tui-overview-filtering.svg)

![Filtering — broader match](images/tui-overview-filtering-demo.svg)

Note: search currently matches against the full resource manifest, not just visible columns. Labels from ArgoCD or other GitOps tools can produce broad matches that select nearly everything. If this affects your workflow, [report an issue](https://github.com/XplorerHQ/xplorer-community/issues) to help refine the behavior.

Select any resource and press `Enter` to drill into its full hierarchy.

## Resource Tree

---

Use arrow keys or `j`/`k` to move through the tree. The full hierarchy unfolds from Claim down through Composite Resources to every Managed Resource underneath.

![Tree Navigation](images/tui-tree-navigation.svg)

Health status sits inline on each row — healthy resources show their Ready/Synced indicators, unhealthy ones are highlighted so your eye goes straight to the problem.

### Live Updates

---

The tree subscribes to the Kubernetes watch stream and updates in real time. When a resource changes, it briefly highlights so you can see exactly what's moving.

![Live Update](images/tui-tree-live-update.svg)

If a status change appears — a resource turning unhealthy, a new error condition — the TUI automatically switches to the error detail view. No manual refresh needed.

## Viewing Manifests

---

Press `Enter` or `Ctrl+D` on any resource to open its YAML in a syntax-highlighted panel.

![Manifest View](images/tui-tree-navigation-details-panel.svg)

Inside the panel, `Ctrl+T` / `Ctrl+P` cycle through syntax themes, `Ctrl+W` toggles word wrap, and `Escape` closes it.

## Copying Resource Details

---

Press `Ctrl+C` on any resource to copy its status, kind, name, and composition to the clipboard — ready to paste into a Slack message, incident ticket, or terminal.

![Copy Resource](images/tui-copy-name-line.svg)

A copied line looks like:

```
✓ ✓    afd-hub    AcmeAFD    atlas-corestack-hub    212d
```

For more control, `Alt+Shift+C` opens an edit panel with the focused content. Use `Shift` + cursor keys to select exactly what you need, then `Ctrl+C` to copy and close (requires the [iTerm2 Alt key fix](#iterm2-alt-key-configuration)).

![Copy Edit Panel](images/tui-copy-adhoc-content.svg)

## Debugging After a Provider Upgrade

---

Provider upgrades can break existing compositions when CRD schemas change. Instead of chasing errors across `kubectl describe` commands, Xplorer surfaces the root cause directly in the tree.

![Error Details](images/tui-tree-navigation-errors.svg)

In this example, a provider upgrade changed the schema for `VirtualNetwork` and the composition still references `.spec.forProvider.dnsServers` — a field no longer declared in the new schema. The tree shows `Synced: False` on the XR with a `ReconcileError`, the exact error message, the affected resource and API version, and downstream effects like `WatchCircuitOpen` from CompositionRevision thrashing. What previously required correlating events across multiple resources is visible in a single screen. Navigate to the affected resource and press `Enter` to inspect its full manifest.

## Pause and Resume Reconciliation

---

During an incident you often need to stop Crossplane from reconciling while you investigate. Press `p` on any resource to pause it — no `kubectl patch` needed.

![Pause Confirmation](images/tui-pause-confirm.svg)

A confirmation dialog shows the resource name, kind, and a clear warning. Once confirmed, the tree reflects the paused state immediately.

![Resource Paused](images/tui-pause-paused.svg)

Under the hood, Xplorer adds the `crossplane.io/paused: 'true'` annotation. Open the manifest to verify — it's visible directly in `metadata.annotations`.

![Paused Annotation in Manifest](images/tui-pause-details.svg)

Press `p` again to resume. For broader control, `Shift+P` cascades the pause across the selected resource and all its children — useful when you need to stabilize an entire claim hierarchy before applying a fix.

## Alternative Views

---

The TUI offers three views beyond the default tree for slicing your cluster from different angles.

### Group by Kind — `Ctrl+K`

---

![Kind View](images/tui-kind-view.svg)

Groups all resources by Kubernetes kind. Useful when you want every instance of a particular resource type across the cluster in one place, regardless of which claim they belong to.

### Group by Namespace — `Ctrl+N`

---

![Namespace View](images/tui-namespace-view.svg)

In Crossplane v2, namespaces often map to environments — dev, staging, production. This view gives you a per-environment slice of cluster health without filtering through unrelated resources.

### Unhealthy Scan — `Ctrl+U`

---

![Unhealthy View](images/tui-unhealthy.svg)

> **Note:** This screenshot uses synthetic test data — 58 unhealthy compositions at once is not something you'd typically see in production.

The scan surfaces two categories that are easy to miss otherwise. The obvious one is XRs or claims reporting `Ready: False` or `Synced: False`. The harder one is a parent that looks healthy — both statuses True — while one or more managed resources underneath are failing or degraded. Crossplane's status propagation doesn't always bubble child failures up to the parent, so without this scan those issues go unnoticed until something breaks harder.

Select any composition and press `Enter` to drill into the full tree.

![Unhealthy Expanded](images/tui-unhealthy-expanded.svg)

In this expanded view, the XR's Ready condition shows reason `Creating` — a managed resource is still being provisioned by the cloud provider. Drilling into Resource Refs reveals the specific `Release` resource with both `Ready: False` and `Synced: False` and its own condition details. One screen, no hidden failures.

## Themes

---

Switch themes with `Alt+T` (next) and `Alt+Shift+T` (previous). The change applies to both the main UI and the syntax highlighting in the manifest panel in a single action. Your selection persists to `~/.config/xplorer/config.yaml`.

![Theme — Tree View](images/tui-theme.svg)

Available themes: xplorer-dark (default), textual-dark, textual-light, nord, gruvbox, catppuccin-mocha, catppuccin-latte, dracula, tokyo-night, monokai, solarized-light, flexoki.

![Theme — Manifest View](images/tui-theme-manifest.svg)

![Theme — Manifest View (alternate)](images/tui-theme-manifest2.svg)

## Help

---

Press `F1` on any screen to open a contextual help modal that lists every available action for the panel you're currently in. Shortcuts adapt to context — the tree, the manifest panel, and other views each show their own relevant actions.

![Help Modal](images/tui-help-modal.svg)

If you forget any shortcut from this guide, `F1` has the answer.

## Universal Resource Palette _(WIP)_

---

`Ctrl+X, R` opens the Universal Resource Palette — a fuzzy search across every Kubernetes resource in the cluster, not just Crossplane resources.

![Universal Resource Palette](images/tui-resource-pallette.svg)

It also handles the kind collision problem introduced in Crossplane v2, where providers register the same kind in both the legacy cluster-scoped API group and the new `.m.` namespaced group — `Bucket` exists in both `s3.aws.upbound.io` and `s3.aws.m.upbound.io` with identical names, which breaks most Kubernetes tooling that assumes kind names are unique. The palette presents each variant with its full API group so you pick exactly the one you need.

This feature is under active development, aiming toward a single entry point for any resource in the cluster with the same manifest viewing and navigation available today.

## Keyboard Reference

---

See the full [Keyboard Shortcuts](keyboard-shortcuts.md) reference for every shortcut across all panels, including chord sequences (`Ctrl+X` prefix).

## iTerm2: Alt Key Configuration

---

Several shortcuts use the `Alt` modifier — theme switching, the copy edit panel. By default, iTerm2 on macOS treats the Option key as a compose key for special characters instead of sending the escape sequence terminal apps expect. Go to **Preferences > Profiles > Keys** and set **Left Option Key** to **Esc+** to fix this.

## Tips

---

Use `Ctrl+R` to refresh the tree after making changes outside Xplorer. Press `F12` to capture an SVG screenshot of the current TUI state to your clipboard. The header auto-hides shortcuts and the logo on narrow terminals to preserve space. Connect to a specific cluster with `xplorer tui --context my-cluster`.
