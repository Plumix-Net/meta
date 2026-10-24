# Plumix Multi-Repo Agents Guide

This folder is an umbrella over several independent git repositories plus the
project's planning documents. Read this file first, then the local `AGENTS.md`
of the repository you are working in.

## 0) What is in this folder

Code repositories (each is its own git repo, ignored by the umbrella repo,
all under `github.com/Plumix-Net` unless noted):

- `Plumix` — the framework: Flutter-like `Widget`/`Element`/`RenderObject`
  layer in C# on .NET 10, Avalonia as platform host. NuGet `Plumix`,
  `Plumix.Material`, `Plumix.Cupertino`, `Plumix.Android`. Solution
  `src/Plumix.slnx`; roadmap — `Plumix/docs/FRAMEWORK_PLAN.md`; developer
  reference — `Plumix/AGENTS.md`; public overview/FAQ — `Plumix/meta/`.
- `Plumix.Packages` — ports of popular Flutter packages (`Plumix.Provider`,
  `Plumix.Bloc`, `Plumix.Slang`), solution `src/Plumix.Packages.slnx`. Switches
  to a project reference on `../Plumix` when the sibling exists
  (`-p:UsePlumixLocal=false` to opt out). See `Plumix.Packages/AGENTS.md`.
- `NetBench` — demo app: HTTP load-testing tool, Avalonia on desktop/browser,
  Plumix on Android/iOS. Consumes Plumix from NuGet
  (`NetBench/NetBench/Directory.Packages.props`). See `NetBench/AGENTS.md`.
- `landing` — site plumix.net. Vite + React + shadcn, prerendered
  (`npm run build`), deployed to Beget from the `release` branch
  (`.github/workflows/deploy-release-beget.yml`).
- `Avalonia` — **upstream clone** of `github.com/AvaloniaUI/Avalonia`
  (checked out at a release branch), read-only reference. `Plumix/avalonia-src`
  is a symlink to it. Never commit or push here.
- `.github` — organization profile; the page lives in `profile/README.md`.

Outside this folder: `Plumix/flutter-src` is a symlink to a local Flutter SDK
checkout (`~/Documents/flutter/flutter`) — the Dart source of truth for ports.

Planning documents (tracked by the umbrella repo):

- `docs/` — strategy, product, marketing. Index with per-document status:
  `docs/README.md`.
- `.claude/launch.json` — dev-server presets for the browser preview
  (`landing-dev`, `landing-preview`, `landing-dist`).

## 1) Ownership and Source of Truth

- Framework behavior lives in `Plumix`; packages in `Plumix.Packages` only
  build on its public API. A framework change needed by a package = Plumix
  change + NuGet release + `PlumixVersion` bump in
  `Plumix.Packages/src/Directory.Packages.props`.
- `NetBench` pins Plumix NuGet versions; bump them after a Plumix /
  Plumix.Packages release.
- All Plumix NuGet packages share one version line with `Plumix.Packages`.
- Product/marketing decisions live in `docs/`; code facts live in each repo's
  `AGENTS.md`. When they disagree, the code wins — fix the doc.

## 2) Standard Commands

```bash
# Framework
cd /Users/egorozh/Plumix/Plumix
dotnet build src/Plumix.slnx
dotnet test src/Plumix.Tests/Plumix.Tests.csproj

# Packages
cd /Users/egorozh/Plumix/Plumix.Packages
dotnet build src/Plumix.Packages.slnx -c Release
dotnet test src/Plumix.Packages.slnx -c Release

# Demo app
cd /Users/egorozh/Plumix/NetBench
dotnet run --project NetBench/NetBench.Desktop/NetBench.Desktop.csproj

# Landing
cd /Users/egorozh/Plumix/landing
npm i
npm run dev
```

## 3) Cross-Repo Change Playbook

1. Change the framework in `Plumix`, keep its tests green, update its
   `CHANGELOG.md` and tracking docs.
2. If `Plumix.Packages` is affected, build it against the local sibling
   (default) before releasing.
3. Release: tag `v*.*.*` in `Plumix`, then in `Plumix.Packages`.
4. Bump versions in `NetBench` and verify it builds.

## 4) Documents

- One document = one question; short kebab-case file names, unique across
  `docs/` — refer to them by file name in backticks.
- A living document carries an "updated"/"актуально на" date in its header.
- Keep the status table in `docs/README.md` current when a document changes.
- Binary sources (screens, mockups) go to `docs/assets/`, not committed.

## 5) Security and Secrets Hygiene

- Never commit real secrets (NuGet API keys, Beget FTP credentials — they
  belong in GitHub Actions secrets only).
- Treat any secret-like value found in local config as sensitive; rotate if
  exposure is suspected.
