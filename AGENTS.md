# AGENTS.md

This is a FoundryVTT module: `ffd20-content-for-pf1e` hosted on GitHub so it can be
installed in Foundry VTT elsewhere via a manifest URL.

## Repository layout

- `module.json` — Foundry module manifest. Version, `url`, `manifest`, `download`
  fields are the source of truth for releases.
- `packs/` — FoundryVTT v12 LevelDB compendium packs. These are binary databases.
  Do NOT regenerate, convert, or hand-edit them.
- `.gitattributes` — marks LevelDB files (`*.ldb`, `*.log`, `CURRENT`, `LOG`,
  `LOG.old`, `LOCK`, `MANIFEST-*`) as binary. `module.json` is text.
- `.gitignore` — excludes LevelDB runtime artifacts (`LOCK`, `LOG`, `LOG.old`).

The project is deliberately **unlicensed** (all rights reserved). Do not add a
LICENSE file unless the user asks.

## Release workflow (semver, current release is alpha: v0.1.0)

A release consists of: version bump in `module.json` -> tag -> push -> build zip
from the tag -> publish GitHub release with the zip as the asset.

1. **Bump version** in `module.json` (e.g. `"version": "0.1.1"`). Keep a stable
   manifest URL convention (see below). Commit: `git add module.json && git commit -m "..." && git push`.

2. **Tag and push** the new version:
   ```
   git tag v<X.Y.Z> && git push origin v<X.Y.Z>
   ```

3. **Build the release zip** from the tag (root contains `module.json`), named
   after the release:
   ```
   git archive --format zip --output /tmp/ffd20-content-for-pf1e-v<X.Y.Z>.zip v<X.Y.Z>
   ```

4. **Create the release** with the zip asset. Choose alpha/prerelease flag based
   on version. Notes must warn when alpha quality. On each release, update the
   previous `download` URL in `module.json` to point at the new asset **before**
   tagging and pushing.

5. **Verify** both URLs return HTTP 200:
   ```
   curl -s -o /dev/null -w "%{http_code}\n" \
     https://raw.githubusercontent.com/maw343/ffd20-content-for-pf1e/v<X.Y.Z>/module.json
   curl -sL -o /dev/null -w "%{http_code}\n" \
     https://github.com/maw343/ffd20-content-for-pf1e/releases/download/v<X.Y.Z>/ffd20-content-for-pf1e-v<X.Y.Z>.zip
   ```

## URL conventions (keep these exact)

- Repo: `https://github.com/maw343/ffd20-content-for-pf1e`
- Manifest: `https://raw.githubusercontent.com/maw343/ffd20-content-for-pf1e/v<X.Y.Z>/module.json`
- Download asset: `https://github.com/maw343/ffd20-content-for-pf1e/releases/download/v<X.Y.Z>/ffd20-content-for-pf1e-v<X.Y.Z>.zip`
- Install in Foundry: Install Module -> paste the manifest URL. Update is detected
  by consulting the manifest URL.

## Environment (the user's machine)

- GitHub CLI is installed at `~/.local/bin/gh` (no sudo; NOT on system PATH by
  default), authenticated as `maw343`.
- Git identity: user `maw343`, email `milkus500@gmail.com`; gh configured as git
  credential helper.
- When replacing a bad release, use:
  ```
  ~/.local/bin/gh release delete v<old> --repo maw343/ffd20-content-for-pf1e --yes --cleanup-tag
  git tag -d v<old>
  ```