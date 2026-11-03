# GanDesktop patches for gpui-fast 0.1.13

The base commit imports the unmodified crates.io gpui-fast 0.1.13 archive,
including its published Cargo manifests and Apache-2.0 license.
Upstream: https://github.com/longbridge/gpui-fast
Branch: `gds-0.1.13`.

The patch is ported from GanDesktop's `gpui-pre` / `gpui-pre-linux`
`gds-0.3.11` branches. It adds external Wayland/X11 parent identifiers and
explicit startup / activation tokens. Platform protocol objects and dispatch
remain inside GPUI. No rendering or layout optimization is changed.

Apply the `gpui-fast` and `gpui-fast-linux` overrides together and enable
`gpui-kit/gpui-fast`. The original package names and version are preserved.

## File-drop input modality

External file Entered, Pending and Submit events restore mouse input modality before dispatch. Previously, dropping files after keyboard input left hover-based drop hit-testing disabled. GDS exercises this through a live PTY test that drops a folder from Files, presses Enter, then drops an external file and verifies both inputs.

## Window clipboard MIME access

Adds explicit MIME offer reads and conditional writes on the active display
connection. Existing text/image clipboard APIs are unchanged. Other backends
report unsupported access by default; the matching Linux patch implements the
Wayland core data-device protocol. Offer ids prevent asynchronous cut cleanup
from overwriting a newer selection.
