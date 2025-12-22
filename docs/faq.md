# FAQ

Frequently asked questions about Xplorer.

## General

### What is Xplorer?

Xplorer is a CLI tool for exploring Crossplane resources. It automatically traces the hierarchy from claims through composite resources (XRs) to managed resources, showing health status and errors along the way.

### What problem does it solve?

When debugging Crossplane, you typically need to run multiple `kubectl` commands to trace from a claim to its XR to the managed resources. Xplorer does this automatically in a single command.

### Is Xplorer open source?

Xplorer is currently closed source. The binary is free for evaluation. Enterprise licensing will be available for production use.

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

**Scoop (Windows):**
```powershell
scoop update xplorer
```

## Usage

### Why can't Xplorer find my claims?

Common causes:
1. **Wrong namespace** - Use `-n <namespace>` or check all namespaces
2. **Missing permissions** - Ensure your kubeconfig has access to claim resources
3. **No claims exist** - Verify claims exist with `kubectl get claims`

### What does "Ready: False" mean?

The resource hasn't reached a healthy state. Use `-v` flag to see error details and related events.

### What does "Synced: False" mean?

The resource's actual state doesn't match its desired state. Crossplane is either still reconciling or encountered an error.

### How do I see more details?

Use verbosity flags:
- `-v` - Shows managed resources with issues
- `-vv` - Shows all managed resources

### Can I use Xplorer with multiple clusters?

Yes, use the `--context` flag:
```bash
xplorer --context production show my-claim
```

## Troubleshooting

### Xplorer is slow on large clusters

Try:
1. Filter by namespace: `xplorer list -n specific-namespace`
2. Target specific claims: `xplorer show specific-claim`

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
