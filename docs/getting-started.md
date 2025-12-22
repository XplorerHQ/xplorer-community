# Getting Started

This guide will help you install Xplorer and explore your first Crossplane claim.

## Prerequisites

- Kubernetes cluster with Crossplane installed
- `kubectl` configured with cluster access
- At least one Crossplane claim in your cluster

## Installation

### macOS / Linux

```bash
brew tap xplorerhq/dist
brew install xplorer
```

### Windows

```powershell
scoop bucket add xplorer https://github.com/XplorerHQ/scoop-xplorer
scoop install xplorer
```

### Verify Installation

```bash
xplorer --version
```

## Your First Exploration

### 1. List Available Claims

See all Crossplane claims in your cluster:

```bash
xplorer list --claims
```

This shows claims across all namespaces with their health status.

### 2. Explore a Claim

Pick a claim and explore its resource hierarchy:

```bash
xplorer show <claim-name>
```

This displays the claim and its immediate composite resource (XR).

### 3. See the Full Hierarchy

Add `-v` to see managed resources:

```bash
xplorer show <claim-name> -v
```

This shows:
- The claim
- The composite resource (XR)
- All managed resources with health status
- Error details for unhealthy resources

### 4. Interactive Mode

Run without arguments for interactive claim selection:

```bash
xplorer
```

Use fuzzy search to find and select a claim.

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

- See [CLI Reference](cli-reference.md) for all commands
- Use `xplorer show <claim> -vv` for maximum verbosity
- Try `xplorer watch <claim>` to monitor changes in real-time
