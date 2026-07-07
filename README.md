# Scoop bucket

Seed files for the [Scoop](https://scoop.sh) bucket that installs Kickoutchi on
Windows. Scoop needs the manifest to live in its own repository, so this folder
is the source you copy into that repo once -- after that, the bucket repo is the
canonical copy (Excavator updates it automatically) and this folder is a
reference that may lag.

## What users run

```powershell
scoop bucket add kickoutchi https://github.com/nuggocto/scoop-bucket
scoop install kickoutchi
```

Installs both `kickoutchi.exe` and `kick.exe` from the release's
`x86_64-pc-windows-msvc` archive. Windows on ARM is not shipped (x64 only).

## One-time bootstrap

1. Create a public repo named `nuggocto/scoop-bucket`.
2. Copy this folder's contents into it so the layout is:
   - `bucket/kickoutchi.json`
   - `.github/workflows/excavator.yml`
3. Push. In the repo's **Settings -> Actions -> General**, allow workflows and give
   them **Read and write permissions** (Excavator commits manifest bumps with the
   repo's own `GITHUB_TOKEN` -- no personal access token needed, unlike the
   Homebrew tap).

## How updates happen

`bucket/kickoutchi.json` carries `checkver` (watch this repo's GitHub releases)
and `autoupdate` (template the archive URL by `$version` and read the hash from
the `.sha256` sidecar every release already publishes). Excavator runs every four
hours, and on a new release it regenerates the manifest -- version, URL, and hash
-- and commits it. No manual step per release, and no hash is ever computed by
hand.

To test a manifest change before pushing:

```powershell
scoop install ./bucket/kickoutchi.json
scoop uninstall kickoutchi
```
