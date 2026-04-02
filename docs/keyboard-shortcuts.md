# Keyboard Shortcuts

Complete reference for all keyboard shortcuts available in the Xplorer TUI.

## Global

These shortcuts are available on every screen.

| Key | Action |
|-----|--------|
| `F1` | Contextual help for current panel |
| `F2` | Toggle header visibility |
| `F12` | Capture SVG screenshot to clipboard |
| `Ctrl+C` | Copy focused content to clipboard |
| `Alt+Shift+C` | Open copy edit panel |
| `Alt+T` | Next color theme |
| `Alt+Shift+T` | Previous color theme |
| `Ctrl+Q` (2x) | Exit (press twice within 3 seconds) |

## Resource Selection (Main Screen)

The initial screen listing all claims and XRs.

| Key | Action |
|-----|--------|
| `Enter` | View resource tree for selected item |
| `Ctrl+D` | Show manifest for selected item |
| `Ctrl+K` | Group by kind |
| `Ctrl+N` | Group by namespace |
| `Ctrl+U` | Unhealthy scan |
| `Ctrl+W` | Toggle watch mode |
| `Ctrl+R` | Refresh resource list |
| `Ctrl+A` | Show all (clear kind/namespace filter) |
| `Ctrl+C` | Copy selected row |
| `Shift+Right/Left` | Toggle detail panel |
| `Escape` | Cancel / collapse detail panel |

## Resource Tree

Displayed after selecting a claim or XR to explore.

| Key | Action |
|-----|--------|
| `Enter` | View manifest for selected resource |
| `Ctrl+D` | View manifest for selected resource |
| `p` | Toggle pause on selected resource |
| `Shift+P` | Toggle pause with cascade (resource + children) |
| `Ctrl+R` | Refresh tree |
| `Escape` | Back to selection |

## Grouped Resource View

Displayed when viewing resources grouped by kind or namespace.

| Key | Action |
|-----|--------|
| `Enter` | Drill down into selected resource |
| `Ctrl+D` | Show manifest |
| `p` | Toggle pause on selected resource |
| `Ctrl+R` | Refresh subtree |
| `Escape` | Back |

## Unhealthy Compositions

Displayed after triggering an unhealthy scan with `Ctrl+U`.

| Key | Action |
|-----|--------|
| `Enter` | Drill down into selected composition |
| `Ctrl+D` | Show manifest |
| `Ctrl+U` | Re-scan for unhealthy resources |
| `p` | Toggle pause on selected resource |
| `Escape` | Cancel scan / back |

## YAML Manifest Viewer

The syntax-highlighted manifest panel opened with `Enter` or `Ctrl+D`.

| Key | Action |
|-----|--------|
| `Alt+T` | Next syntax theme |
| `Alt+Shift+T` | Previous syntax theme |
| `Ctrl+W` | Toggle word wrap |
| `Ctrl+C` | Copy full YAML to clipboard |
| `Alt+Shift+C` | Open copy edit panel for selective copy |
| `Up/Down` | Scroll line by line |
| `Left/Right` | Scroll horizontally |
| `Page Up/Down` | Scroll by page |
| `Home/End` | Jump to top / bottom |
| `Escape` | Close manifest panel |

## Universal Resource Browser

Available when browsing resources via chord navigation (`Ctrl+X, R`).

| Key | Action |
|-----|--------|
| `Enter` | View manifest |
| `Ctrl+D` | View manifest |
| `Ctrl+N` | Filter by namespace |
| `Shift+Right/Left` | Toggle detail panel |
| `Escape` | Cancel / collapse / back |

## Copy Edit Panel

Opened with `Alt+Shift+C` for selective content copying.

| Key | Action |
|-----|--------|
| `Shift+Arrow keys` | Select text |
| `Ctrl+C` | Copy selection and close panel |
| `Escape` | Cancel |

## Pause Confirmation Dialog

Shown when pressing `p` or `Shift+P` on a resource.

| Key | Action |
|-----|--------|
| `Enter` | Confirm pause/resume |
| `Escape` | Cancel |

## Chord Shortcuts (Ctrl+X prefix)

Press `Ctrl+X` first, then the second key within the chord timeout.

| Chord | Action |
|-------|--------|
| `Ctrl+X, R` | Browse all resources (Universal Resource Browser) |
| `Ctrl+X, Shift+R` | Force refresh Universal Resource Browser |
| `Ctrl+X, N` | Next steps |
| `Ctrl+X, U` | Navigate up |
| `Ctrl+X, C` | Copy focused content |
| `Ctrl+X, Shift+C` | Open copy edit panel |
| `Ctrl+X, Ctrl+C` | Copy full tree |
| `Ctrl+X, Alt+C` | Open copy edit panel with full tree |
