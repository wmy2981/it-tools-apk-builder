# it-tools-apk-builder

> **Unofficial** automated Android APK builder for [CorentinTh/it-tools](https://github.com/CorentinTh/it-tools).
> No affiliation with the it-tools authors. This is a derived work, not an official product.

Automatically packages [IT Tools](https://it-tools.tech) — a collection of handy online tools for developers — into an installable Android APK, following the official release process.

## How it works

| Workflow | Trigger | Job |
|---|---|---|
| `build.yml` | `schedule` every Monday 08:00 UTC, plus manual `workflow_dispatch` | Polls the official `CorentinTh/it-tools` releases API. When a new stable tag (e.g. `v2024.10.22-7ca5933`) is found, checks out that exact tag, builds the web app (`vue-tsc + vite`), syncs it into the Capacitor Android shell, and builds a debug-signed APK |
| `release.yml` | `workflow_run` (after `build.yml` succeeds) | Creates a GitHub Release tagged `v<official-tag>-android` with the APK and its SHA-256 checksum as release assets |

- **Idempotent**: if the release `v<tag>-android` already exists, no rebuild happens.
- **First run**: trigger `build.yml` manually via the Actions tab — it will build the current latest official version immediately.
- **Versioning**: our release tag = official tag + `-android` suffix, so each APK maps 1:1 to an upstream version.
- **Icon**: the official it-tools icon is pulled from the upstream repo at build time.

## Install

1. Download the APK from the [Releases](../../releases) page.
2. On your phone, allow installing from unknown sources.
3. Open the APK and install. All tools run locally in the app — no server, no tracking.

## License & compliance

This project is licensed under the **GNU GPL v3**, the same license as its upstream:

- Upstream: [CorentinTh/it-tools](https://github.com/CorentinTh/it-tools) (© Corentin Thomasset, GPLv3) — see [LICENSE](LICENSE).
- Per GPLv3, the full corresponding source of every release is available:
  - Web app source: the official upstream repo at the exact tag named in the release
  - Packaging source (Capacitor shell + workflows): this repository
- These APKs are unofficial builds of the upstream project. Please don't direct issues/feature requests here — report them to the [upstream issue tracker](https://github.com/CorentinTh/it-tools/issues).
