# PineHook Releases

This folder holds the official SHA-256 hashes for every PineHook Expert
Advisor and DLL release. The binaries themselves are attached to tagged
GitHub Releases on this same repo. Together, those two things let anyone
who downloads a PineHook binary confirm it hasn't been tampered with.

## Files

- **`SHA256SUMS.txt`**: append-only history of every release ever
  published. Standard `sha256sum` output format, so it works directly with
  `sha256sum --check`.
- **`LATEST.txt`**: only the two currently-served release lines (EA and
  DLL). Overwritten on every release.

## How to verify a downloaded binary

Full guide: [pinehook.io/docs/security/verifying-downloads](https://pinehook.io/docs/security/verifying-downloads).

Quick version:

```bash
# macOS / Linux
sha256sum PineHook-2.4.1.ex5

# Windows PowerShell
Get-FileHash -Algorithm SHA256 PineHook-2.4.1.ex5
```

Or verify every downloaded binary in one shot against the official record:

```bash
curl -O https://raw.githubusercontent.com/PineHook/TradingView-Webhook-Relay/main/releases/SHA256SUMS.txt
sha256sum --check SHA256SUMS.txt
```

`sha256sum --check` skips files you don't have locally and errors loudly
on any mismatch.

## How releases reach this folder

Every PineHook release is cut from a maintainer's machine using a private
tool that lives outside this repo. When a new version goes out, that tool:

- Cuts a signed tag (`ea-v<version>` or `dll-v<version>`).
- Creates a GitHub Release on that tag with two assets: the binary and a
  per-release `SHA256SUMS` file.
- Appends the new hash line to `SHA256SUMS.txt` in this folder.
- Overwrites `LATEST.txt` with the current EA and DLL pointers.
- Commits and pushes both files to `main`.

Releases are never force-pushed, and entries in `SHA256SUMS.txt` are
never edited after the fact.

## Why three independent publication surfaces

Each release's hash ends up in three places on GitHub:

1. `SHA256SUMS.txt` on `main`: append-only history, visible in `git log`
   forever.
2. `LATEST.txt` on `main`: current pointers only.
3. The tagged GitHub Release's `SHA256SUMS` asset: immutable per tag.

An attacker who wants to forge a PineHook binary that verifies cleanly
has to tamper all three of those surfaces AND the dashboard manifest on
pinehook.io, without leaving an audit trail. That is a much harder
target than a single-source distribution, and it's why we publish across
both infrastructures.
