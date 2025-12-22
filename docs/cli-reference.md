# CLI Reference

Complete reference for all Xplorer commands and options.

## Global Options

These options work with all commands:

| Option | Description |
|--------|-------------|
| `--context` | Kubernetes context to use |
| `--debug` | Enable debug logging (creates log file) |
| `--help` | Show help message |
| `--version` | Show version |

## Commands

### `xplorer` (default)

Interactive mode - select a claim with fuzzy search.

```bash
xplorer
xplorer --claims      # Show only claims
xplorer --xr          # Show only XRs
```

### `xplorer show <name>`

Explore a specific claim or XR hierarchy.

```bash
xplorer show my-claim
xplorer show my-claim -v      # Include managed resources
xplorer show my-claim -vv     # Maximum verbosity
xplorer show my-claim -n dev  # Specify namespace
```

| Option | Short | Description |
|--------|-------|-------------|
| `--namespace` | `-n` | Namespace to search in |
| `--verbose` | `-v` | Increase verbosity (use -v or -vv) |

### `xplorer list`

List resources without exploring hierarchy.

```bash
xplorer list              # List all (claims + unmatched XRs)
xplorer list --claims     # List claims only
xplorer list --xr         # List XRs only
```

| Option | Description |
|--------|-------------|
| `--claims` | Show only claims |
| `--xr` | Show only composite resources |
| `--namespace` | Filter by namespace |

### `xplorer watch <name>`

Watch a claim for real-time changes.

```bash
xplorer watch my-claim
xplorer watch my-claim -n dev
```

Press `Ctrl+C` to stop watching.

## Verbosity Levels

| Level | Flag | Shows |
|-------|------|-------|
| Default | (none) | Claim + immediate XR |
| Verbose | `-v` | + Managed resources with issues |
| Very Verbose | `-vv` | + All managed resources (even healthy) |

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

### Watch infrastructure changes

```bash
xplorer watch my-vpc -v
```

Real-time updates as resources change.

### Use specific Kubernetes context

```bash
xplorer --context production-cluster show my-claim
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
