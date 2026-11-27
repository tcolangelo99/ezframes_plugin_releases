# Security policy

## Reporting a vulnerability

Please report security problems privately by email to **ezframes.app@gmail.com**, not in a public issue.
Say what you found and how to reproduce it; we answer as soon as we can and credit you if you wish.

## How releases are protected

- Every plugin release manifest is signed with an offline key whose public half is pinned in the updater. The updater,
  the installer's download step and Repair install a release only when that signature verifies, and every file they
  download must match the SHA-256 the signed manifest lists for it.
- From v0.1.129, each release's notes give the installer EXE's SHA-256 (check a download with
  `Get-FileHash .\EzFramesPluginInstaller-v<version>.exe` in PowerShell), and each release carries a CycloneDX SBOM
  (`ezframes-plugin-sbom-v<version>.cdx.json`).
