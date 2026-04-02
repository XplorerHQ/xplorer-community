# CLI Reference

Complete reference for all Xplorer commands and options.

## Global Options

These options work with all commands:

| Option | Description |
|--------|-------------|
| `--context` | Kubernetes context to use |
| `--cache-server` | Address of an xplorer-cache server (e.g., `http://localhost:8443`) |
| `-n, --namespace` | Namespace to search in |
| `-v, --verbose` | Show all managed resources in the hierarchy |
| `--claims` | Show only Claims |
| `--xr` | Show only XRs (mutually exclusive with `--claims`) |
| `--debug` | Enable debug logging (creates log file) |
| `--help` | Show help message |
| `--version` | Show version |

## Commands

### `xplorer show [name]`

Explore a specific claim or XR hierarchy. When no name is provided, launches interactive fuzzy search.

```bash
xplorer show my-claim
xplorer show my-claim -v       # Include managed resources
xplorer show my-claim -v       # Show all managed resources
xplorer show my-claim -n dev   # Specify namespace
xplorer show                   # Interactive selection
```

| Option | Short | Description |
|--------|-------|-------------|
| `--namespace` | `-n` | Namespace to search in |
| `--verbose` | `-v` | Show all managed resources |
| `--claims` | | Show only claims |
| `--xr` | | Show only XRs |
| `--strict-case-match` | | Require exact case match for resource names |

### `xplorer list`

List resources without exploring hierarchy.

```bash
xplorer list              # List all (claims + unmatched XRs)
xplorer list --claims     # List claims only
xplorer list --xr         # List XRs only
xplorer list --unhealthy  # List only unhealthy resources
xplorer list -k RDSInstance  # Filter by kind
```

| Option | Short | Description |
|--------|-------|-------------|
| `--claims` | | Show only claims |
| `--xr` | | Show only composite resources |
| `--namespace` | `-n` | Filter by namespace |
| `--kind` | `-k` | Filter by resource kind |
| `--unhealthy` | | Show only unhealthy resources |
| `--verbose` | `-v` | Increase verbosity |

### `xplorer watch <name>`

Watch a claim for real-time changes.

```bash
xplorer watch my-claim
xplorer watch my-claim -n dev
xplorer watch my-claim --period 10    # Poll every 10 seconds
xplorer watch my-claim --timeout 30   # Stop after 30 minutes
```

| Option | Short | Description |
|--------|-------|-------------|
| `--namespace` | `-n` | Namespace to search in |
| `--period` | | Polling interval in seconds |
| `--timeout` | | Stop watching after N minutes |
| `--verbose` | `-v` | Increase verbosity |

Press `Ctrl+C` to stop watching.

### `xplorer tui`

Launch the interactive Terminal UI.

```bash
xplorer tui
```

The TUI provides a full interactive experience with tree navigation, manifest viewing, and resource management. See [TUI Quick Start](getting-started.md) for details.

### `xplorer history`

Show command history.

```bash
xplorer history
```

### `xplorer cache`

Manage the local resource cache.

```bash
xplorer cache show    # Display cache information
xplorer cache clear   # Clear all cached data
```

## Entry Points

| Command | Description |
|---------|-------------|
| `xplorer` | Main CLI |
| `xplorer-tui` | Direct TUI launcher (alias for `xplorer tui`) |

## Verbosity Levels

| Level | Flag | Shows |
|-------|------|-------|
| Default | (none) | Claim + XR, unhealthy managed resources only |
| Verbose | `-v` | Full hierarchy including all managed resources |

## Examples

### Debug a failing claim

```bash
xplorer show my-database -v
```

Shows the hierarchy with error details for unhealthy resources.

### Find all claims in a namespace

```bash
xplorer list --claims -n production
```

### Find unhealthy resources

```bash
xplorer list --unhealthy
```

### Watch infrastructure changes

```bash
xplorer watch my-vpc -v
```

Real-time updates as resources change.

### Use specific Kubernetes context

```bash
xplorer --context production-cluster show my-claim
```

### Use with xplorer-cache server

```bash
xplorer --cache-server http://localhost:8443 list --claims
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Error (check output for details) |

## Debug Logging

Enable debug mode to create a log file:

```bash
xplorer --debug show my-claim
```

Logs are written to: `~/.xplorer/logs/xplorer_debug_YYYYMMDD_HHMMSS.log`

## Environment Variables

| Variable | Description |
|----------|-------------|
| `KUBECONFIG` | Path to kubeconfig file (standard kubectl variable) |
