# CADCraft cheat sheet (OwnerAutomate ep 029, 12 October 2026)

CADCraft is a free, open-source, AutoCAD-style 2D drafting app written in Rust by the ArtCraft team.
Repo: https://github.com/storytold/cadcraft · Downloads: https://github.com/storytold/cadcraft/releases/latest

This is an overview. Everything below comes from the project's README, the v0.6.2 release notes and GitHub release data; nothing was benchmarked.

## Download (pick your system)

| System | File to grab from the latest release |
|---|---|
| Windows 64-bit | `cadcraft-VERSION-windows-x64.msi` (installer) or `-x64-portable.zip` |
| Windows on ARM | `cadcraft-VERSION-windows-arm64.msi` or `-arm64-portable.zip` |
| Mac (Apple silicon + Intel) | `cadcraft-VERSION-macos-universal.dmg` (signed and notarized) |
| Linux, any distro | `cadcraft-VERSION-linux-x86_64.AppImage` (make it executable, then run it) |
| Ubuntu/Debian | `.deb` · Fedora/openSUSE: `.rpm` · Flatpak: `flatpak install --user FILE.flatpak` |
| FreeBSD | `cadcraft-VERSION-freebsd-x86_64.tar.gz` |
| Browser | `cadcraft-web-VERSION.zip`, host on any static web server |

Windows silent install with a desktop shortcut (README): `msiexec /i cadcraft-VERSION-windows-x64.msi /qn DESKTOPSHORTCUT=1`
Windows app won't open (README): set `WGPU_BACKEND` to `vulkan` before launching.
Check downloads against `SHA256SUMS.txt` in the release.

From source (README): `git clone https://github.com/storytold/cadcraft && cd cadcraft && cargo run --release -p cadcraft -- --sample` (desktop build needs Rust 1.95+).

## First commands to type (README quick start)

```text
line 0,0 @10,0 @0,5 c     a closed triangle
circle 5,2 1              a circle
offset 0.25               then pick the circle and a side
zoom e                    zoom to extents
```

## Files

- DXF: opens R12 to 2018 (ASCII and binary). README says it saves R2000; the v0.6.2 release notes add saving DXF/DWG at a chosen version 2000 to 2018.
- DWG: opens R13 to 2018 through the open-source acadrust library.
- Output: PDF plotting, SVG, PNG.
- **Save warning:** the README says entities CADCraft doesn't model yet are not kept on save. The v0.6.2 notes say DXF saves now keep them ("keep what we don't model on save"). The docs lag the code, so always work on a copy of a client's DWG.
- v0.6.2 also added autosave and drawing recovery after a crash.

## For AI agents (README)

- `cadcraft-cli mcp` starts an MCP server; `cadcraft-cli mcp --connect 127.0.0.1:7979` bridges to the running app started with `--control 7979`.
- `cadcraft-cli run --sample --script 'CIRCLE 22,3 1\n' --save out.dxf --export out.png`
- `cadcraft-cli info drawing.dxf` · `cadcraft-cli convert drawing.dxf drawing.svg`

## Cost and trust

- Free: MIT or Apache-2.0, no account, no subscription.
- Windows installers code-signed; macOS signed and notarized.
- Not affiliated with Autodesk.

## Limits

- Status: early development (repo created 7 October 2026; 5 releases from 0.1.0 to 0.6.2 by 12 October; over 200 open issues and pull requests).
- SHX fonts not read yet.
- The README feature list is 2D drafting; no 3D modeler is listed.

## Verdict for a small business

Install it free as a DWG/DXF viewer, PDF plotter and quick 2D sketch tool. Don't cancel AutoCAD, and don't keep the only copy of a client drawing in it, yet.
