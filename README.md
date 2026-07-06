# Corylus — public release distribution

This repository is **public on purpose**: it hosts the built, signed Corylus
installers and the `latest.json` manifest that the in-app auto-updater reads.
The application source lives in the private `corylus` repository, which
includes this repo as a git submodule at `releases/`.

## How the auto-updater uses this repo

The shipped app polls:

```
https://raw.githubusercontent.com/LudoBermejoES/corylus-releases/main/latest.json
```

`latest.json` lists, per platform, the download URL of the installer and its
minisign `signature`. The app downloads the installer and verifies the
signature against the public key baked into `tauri.conf.json` before applying
the update. Only artifacts signed with the project key are accepted.

## Contents

- `latest.json` — updater manifest (version, notes, per-platform url + signature)
- `Corylus_<version>_x64-setup.exe` — Windows NSIS installer
- `Corylus_<version>_x64-setup.exe.sig` — its minisign signature

## Publishing a new version

See `doc/RELEASING.md` in the private `corylus` repo. In short: build a signed
installer, copy the `.exe` (+ `.sig`) here, regenerate `latest.json`, and push
`main`. `raw.githubusercontent.com/.../main/latest.json` always resolves to the
newest committed manifest.
