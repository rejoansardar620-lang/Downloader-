# SOURCE DOCUMENT SNAPSHOT — Part 3 of 3

Dated: 2026-11-16. Original documents reproduced from GitHub. These are immutable historical snapshots, not live current-state claims.

---

# SOURCE 17: docs/REUSE_DECISION_MATRIX.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/REUSE_DECISION_MATRIX.md  
Blob: `c5705463012a70a5c791b2083b566213dc807467`

<!-- BEGIN docs/REUSE_DECISION_MATRIX.md -->

# M0 reuse decision matrix — 17 source-code candidates

**Reviewed:** 15 November 2026. **Decision status:** engineering recommendation, **NOT owner approval**. Exactly **two primary foundations** are recommended. The other 15 are selective dependencies, deferred connectors, or study references; none is imported into Downloader.

## Evidence standard and limitations

- Read the actual repository files using a pinned GitHub default-branch commit, with build manifests, relevant implementation modules, and/or source test directories. This is **source triage**, not a complete 17-project security, licensing, binary, or quality certification.
- Checked default-branch commit, repository metadata (license field, archived status, language, commit date), root layout, and selected source/build files for all 17. Full third-party dependency license inventories, SBOMs, automated tests, binaries, and runtime compatibility are **not yet verified** for those projects.
- Separately cloned and read source for AB desktop, AB extension, ReClip, and MediaGrabber. **Only the AB extension** was bundled and typechecked in this pass; AB desktop Gradle launch failed. No site-specific extraction tests were run.
- The GitHub license field can be NOASSERTION even when source contains license documents. A root license cannot settle obligations of bundled binaries. Activity indicates maintenance interest, not a passing build.
- Use links below as **primary source evidence**, not as code-import authorization. Consult [legal/security requirements](LEGAL_SECURITY.md) and [first audit](M0_AUDIT.md).

## Exactly two selected primary foundations

### 1. AB Download Manager — REUSE/ADAPT, conditional

- **Pinned source:** [amir1376/ab-download-manager at 68f0ea6](https://github.com/amir1376/ab-download-manager/commit/68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68), Apache-2.0 root LICENSE, Kotlin/Kotlin Multiplatform + Compose.
- **Actual code:** downloader/core/src/commonMain/.../DownloadManager.kt persists jobs via IDownloadListDb and supports path/filename validation and collision strategies. queue/QueueManager.kt loads persisted queues. desktop/app/build.gradle.kts wires the engine, monitor, shared app, native messaging launcher and integration server. The downloader/core module uses OkHttp and HLS parser.
- **Existing integration:** integration/server/.../Endpoints.kt provides /add, /ping, /queues; desktop native messaging host writes Chrome allowed_origins and Firefox allowed_extensions with configured IDs.
- **Build risk:** wrapper requests Gradle 9.8; gradle.properties specifies jvm.toolchain=25 (the current cloud image provides JDK 21). Wrapper bootstrap cannot reach the binary release asset from this environment. No desktop binary or Windows demonstration exists here.
- **Security debt:** default loopback HTTP API enabled, API key enforcement disabled; /add supports silent options through the UI integration path. Must harden before public distribution.
- **Why chosen:** reuse one coherent direct-transfer engine, desktop UI, persistence and native companion instead of rebuilding a general downloader in a new framework.

### 2. AB Download Manager Browser Integration — REUSE/ADAPT, conditional

- **Pinned source:** [amir1376/ab-download-manager-browser-integration at bf6811d](https://github.com/amir1376/ab-download-manager-browser-integration/commit/bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05), Apache-2.0 root LICENSE, TypeScript/React/Vite.
- **Actual code:** src/manifest/manifest.chrome.ts emits Manifest V3; src/backend/Backend.ts selects HTTP or native messaging; src/backend/HttpApi.ts calls localhost /add and /ping; src/backend/nativemessaging/NativeMessagingTransport.ts implements message IDs/timeouts; src/linkgrabber/DownloadLinkInterceptor.ts already recognizes HLS .m3u8 requests.
- **Build evidence:** npm ci PASS; Chrome production build PASS; Firefox production build PASS. Standalone TypeScript check FAILS due an unresolved NativeMessaging import, Manifest author type mismatch, and webext-bridge / type-fest declarations.
- **Security debt:** all-host content scripts, broad host_permissions and cookies/webRequest/nativeMessaging permissions need minimization and justification. HTTP fallback prioritizes loopback when native is not connected.
- **Why chosen:** already speaks AB's protocol. Replacing it with MediaGrabber would require a second helper protocol plus extra native executables.

**These two are a compatible candidate pair, not a proved integrated release.** No source-code import until license/brand/platform choices are approved, third-party notices reviewed, and the owner approves the selected commits.

## Full decision matrix (all 17)

Key: REUSE = proposed core foundation; ADAPT = later selective dependency/module after review; REFERENCE ONLY = research architecture/UX, not copying code; DEFER = hold pending scope/policy; REJECT = not suitable as a primary foundation.

| # | Project | Actual implementation/build evidence inspected | Root-license evidence | Decision and rationale |
| --- | --- | --- | --- | --- |
| 1 | [AB Download Manager](https://github.com/amir1376/ab-download-manager) | Kotlin Gradle, downloader/core DownloadManager and QueueManager; persisted DB, native host | Apache-2.0 LICENSE | **REUSE** primary desktop. Build/toolchain/security gates remain. |
| 2 | [AB Browser Integration](https://github.com/amir1376/ab-download-manager-browser-integration) | TS/React/Vite, MV3 manifest, native + HTTP adapters, HLS interception; two bundler builds PASS, typecheck FAIL | Apache-2.0 LICENSE, package.json | **REUSE** primary extension. Maintain protocol compatibility with #1. |
| 3 | [ReClip](https://github.com/averygan/reclip) | Flask app.py subprocesses yt-dlp on submitted URL; in-memory jobs; Dockerfile installs FFmpeg | MIT LICENSE | **REFERENCE ONLY** web UI. Current backend has no visible auth, strict URL/SSRF control, quotas or durable jobs. Unsafe as a hosted default. |
| 4 | [MediaGrabber](https://github.com/miroshArtem/MediaGrabber) | TS MV3 manifest, injected main-world script, Node coapp native host; THIRD_PARTY_NOTICES.md | MIT LICENSE; bundled FFmpeg Windows GPLv3 per notices | **REFERENCE ONLY** for selected discovery/native patterns. Broad page permissions and extra executable/licensing surface. |
| 5 | [HLS Downloader](https://github.com/puemos/hls-downloader) | TS/pnpm workspaces, src/core use-cases for playlist/segments, core Vitest/typecheck scripts, MV2/MV3 build variants | MIT LICENSE and core package | **ADAPT LATER** for authorized HLS tests if AB's existing handling is insufficient; no duplication without measured gap. |
| 6 | [Parabolic](https://github.com/NickvisionApps/Parabolic) | C#/.NET 10 Shared, GNOME/WinUI solutions; DownloadService and yt-dlp dependency/service | MIT LICENSE | **REFERENCE ONLY** desktop video UX alternative. New .NET UI stack plus yt-dlp-first workflow costs more than current AB pair. |
| 7 | [FFmpeg](https://github.com/FFmpeg/FFmpeg) | C, configure, libavcodec/libavformat, tests; LICENSE.md enumerates optional GPL components | LGPL 2.1+ baseline, GPL when enabled; binary-dependent | **ADAPT LATER** as trusted external processor for owned media, with binary provenance and license review. Not a product base. |
| 8 | [yt-dlp](https://github.com/yt-dlp/yt-dlp) | Python package pyproject.toml, yt_dlp implementation and test suite | Unlicense core; optional/bundled libraries differ | **DEFER** rights-reviewed connector, not universal public-site support or Chrome-store shortcut. |
| 9 | [Gopeed](https://github.com/GopeedLab/gopeed) | Go 1.26 module, pkg/download state/persistence, many _test.go files, Flutter/Dart UI | GPL-3.0 LICENSE | **REFERENCE ONLY** engine/API/extension architecture; new Go/Flutter stack plus copyleft source obligations. |
| 10 | [Motrix](https://github.com/agalwood/Motrix) | Electron/TS, pnpm, native messaging bridge tests and desktop/server packaging scripts | package.json MIT; repository license API NOASSERTION; aria2/bundles must be audited | **REFERENCE ONLY** mature UI, bridge tests, release workflows. Avoid second Electron core and unclear bundled-code obligations. |
| 11 | [aria2](https://github.com/aria2/aria2) | C++ DownloadEngine.cc, CLI/JSON-RPC engine, src/test directories | GPL-2.0 COPYING | **REFERENCE ONLY** segmented transfers and RPC, not embed in an unapproved proprietary binary. |
| 12 | [Cobalt](https://github.com/imputnet/cobalt) | Svelte/web, Node/Express API src/cobalt.js and security modules | AGPL-3.0 LICENSE | **REFERENCE ONLY** simple web UX and API boundaries; AGPL/network obligations and different product scope. |
| 13 | [MeTube](https://github.com/alexta69/metube) | Python app, yt-dlp worker, app/url_guard.py with SSRF protections and documented limitations, tests directory | AGPL-3.0 LICENSE | **REFERENCE ONLY** job UI and SSRF lessons; no hosted import under unresolved license. |
| 14 | [Pinchflat](https://github.com/kieraneglin/pinchflat) | Elixir/Phoenix mix.exs, Oban jobs, Ecto/SQLite, test directory | AGPL-3.0 LICENSE | **REFERENCE ONLY** scheduling and library workflow; service architecture is out of M1 scope. |
| 15 | [Tube Archivist](https://github.com/tubearchivist/tubearchivist) | Django 6, Celery, Redis, backend/download/views.py; authenticated views and download tests | GPL-3.0 LICENSE | **REFERENCE ONLY** indexed media library and administrative workflows; heavyweight hosted components. |
| 16 | [YoutubeDL-Material](https://github.com/Tzahi12345/YoutubeDL-Material) | Node/Express backend + web UI, backend/package.json test script, Node 16 engine and older dependencies | MIT LICENSE | **DEFER** historical full-stack UX. Older runtime/dependency set creates update and security cost. |
| 17 | [yt-dlp-web-ui](https://github.com/marcopiovanello/yt-dlp-web-ui) | Go 1.26, server/rpc/service.go and persistent queue/RPC model | GPL-3.0 LICENSE; GitHub archived=true | **REJECT as foundation** (archived/copyleft); source may be studied for ideas but not integrated. |

## Reproducible commit pins — default branches observed 15 November 2026

The short SHAs used in prose are not sufficient for source import: use these **full** commits. A newer default-branch HEAD is not an implicit upgrade.

| # | Repository | Observed exact default-branch commit |
| --- | --- | --- |
| 1 | amir1376/ab-download-manager | 68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68 |
| 2 | amir1376/ab-download-manager-browser-integration | bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05 |
| 3 | averygan/reclip | 1d161d15a4fe93d9b3371377f0a421dc3e965b10 |
| 4 | miroshArtem/MediaGrabber | daf968e2f9ad853b318a10196f0e21c586798db5 |
| 5 | puemos/hls-downloader | 21ba8008a545843b9b34ff709614133873dc59a7 |
| 6 | NickvisionApps/Parabolic | 1118e6a3abbf5ffc1f764c4c9781f98d4c0482d4 |
| 7 | FFmpeg/FFmpeg | 0834db516348d253b2c51f969188501d324c698d |
| 8 | yt-dlp/yt-dlp | ee2559128c7ddc471de5aeaa584a9a4ae6836ec3 |
| 9 | GopeedLab/gopeed | be0a395447d788419e83acdfd693e48a1a1706bf |
| 10 | agalwood/Motrix | 8e12a8d0d84fbb87c3b12dc25df4834286bb0eaf |
| 11 | aria2/aria2 | 9e7273583f83e881e3ec067b523ba88724088d2f |
| 12 | imputnet/cobalt | a636575b09de1fc55d9b8cd98cac88f5f2f16b42 |
| 13 | alexta69/metube | 7746086e0e920925cb14c8f4843084e29548a877 |
| 14 | kieraneglin/pinchflat | 67d8bd5598f4461c5435bcc66fc8d02b00c607cb |
| 15 | tubearchivist/tubearchivist | 5a18613136122dd8fe83d43d95ea1b8af00d79eb |
| 16 | Tzahi12345/YoutubeDL-Material | 28234c0d4ba65059be18f3692f6f5e5b1170b1a6 |
| 17 | marcopiovanello/yt-dlp-web-ui | c7ad3bd79c7c520a7d17e7f2ba19d962be8e7897 |

**Activity nuance:** GitHub repo-level pushed_at may reflect other branches; the pins above use the actual default-branch commit response. Example: Parabolic's inspected default-branch commit is older than the repo's most recent pushed_at. The archived status of #17 was directly verified.

## Maintenance and release signals (GitHub API, 15 November 2026)

These are API metadata observations, **not evidence of security patching or successful releases**. The pushed date is repository-wide and may reflect a non-default branch. The tag/date are from GitHub's latest-release endpoint; *not returned* means the endpoint did not return an identifiable release in this inspection, not that upstream has no tagged releases. A release may refer to code other than the default-branch source pin above.

| # | Repo pushed (UTC date) | Latest-release tag returned | Release date (UTC) | Archived |
| --- | --- | --- | --- | --- |
+| 1 | 2026-11-06 | v1.11.4 | 2026-11-03 | no |
| 2 | 2026-10-31 | v1.5.0 | 2026-07-27 | no |
| 3 | 2026-07-09 | not returned | — | no |
| 4 | 2026-08-11 | v1.1.1 | 2026-08-11 | no |
| 5 | 2026-11-10 | v5.6.0 | 2026-10-31 | no |
| 6 | 2026-11-15 | 2026.5.0 | 2026-05-24 | no |
| 7 | 2026-11-15 | not returned | — | no |
| 8 | 2026-11-14 | 2026.10.26 | 2026-10-26 | no |
| 9 | 2026-11-15 | v1.9.3 | 2026-03-18 | no |
| 10 | 2026-11-15 | v2.4.2 | 2026-11-15 | no |
| 11 | 2026-06-25 | release-1.37.0 | 2023-11-15 | no |
| 12 | 2026-04-06 | not returned | — | no |
| 13 | 2026-11-14 | 2026.11.14 | 2026-11-14 | no |
| 14 | 2025-12-16 | v2025.9.26 | 2025-09-26 | no |
| 15 | 2026-10-25 | v0.6.0 | 2026-10-25 | no |
| 16 | 2026-03-08 | v4.3.2 | 2023-05-26 | no |
| 17 | 2026-08-21 | v4.0.0 | 2026-06-29 | yes |

**Maintenance implications:** AB desktop has a recent release, while the extension's latest-release endpoint returned an older tag despite newer pinned source; confirm **protocol compatibility for the exact commits**. The age of aria2's GitHub release and YoutubeDL-Material's older Node runtime do not automatically establish vulnerabilities; they increase verification burden. The archived yt-dlp-web-ui is unsuitable as a primary maintained foundation.

## Feature ownership: reuse, adapt, build or defer

| Initial requirement | Upstream evidence | Work in Downloader | Decision |
| --- | --- | --- | --- |
| HTTP(S) engine, multi-part download and resume | AB downloader/core download manager, HTTP connection/destination and parts | Targeted fixture-driven tests, confirm range fallback and checksum | **REUSE + VERIFY** |
| Queue, concurrency, scheduling and restart history | AB queue manager with persisted DB, default settings and downloadable jobs | Measure recovery, schedule behavior, invalid state | **REUSE + VERIFY** |
| Download path and naming | AB DownloadManager validates path/filename and collision choice | Threat tests for traversal, platform names, disk full | **ADAPT** |
| Browser context menu / handoff | AB extension existing code and desktop native host | Version compatibility, acceptance demo, negative tests | **REUSE + HARDEN** |
| Native messaging extension-ID whitelist | AB desktop native messaging manifest source | Brand-specific IDs, user pairing and message validation tests | **ADAPT** |
| Local HTTP bridge and silent add | AB optional API-key checks and extension HTTP-first fallback | Require secure default; guard against web-origin abuse and unauthorized silent jobs | **BUILD SECURITY CHANGE** |
| Browser permissions | AB extension all-host scripts, cookies/webRequest/nativeMessaging | Narrow hosts and permissions, justify UX and consent | **ADAPT** |
| Checksums and integrity declaration | Not yet proven as an end-to-end AB UI acceptance flow | SHA-256 fixture assertion, atomic output verification | **BUILD/VERIFY** |
| Controlled HTTPS and disconnect test infrastructure | Our Python local fixture | HTTPS, If-Range, abrupt EOF, CI tests now implemented | **BUILD — FIXTURE ONLY** |
| HLS detection/stream handling | AB extension .m3u8 and desktop HLS credentials already present | Test permitted fixture before evaluating another HLS codebase | **REUSE + VERIFY** |
| MP4/MP3/containers/subtitles | FFmpeg code and license docs | Safe separate process, bounded jobs, format tests and distribution review | **ADAPT LATER** |
| Web interface/backend | ReClip offers a minimal example but accepts arbitrary URLs and in-memory jobs | Original product website later; SSRF/auth/quotas before hosted jobs | **DEFER** |
| Site-specific extraction | yt-dlp offers extractors, with permissions/law/policy constraints | Explicit rights/source policy and regression tests | **DEFER** |
| Advanced catalog, AI, billing, cross-device sync | No selected core baseline | Demand/cost/security decisions after beta | **DEFER** |

## Distribution / license / security risk register

| Risk | Current evidence | Required before release |
| --- | --- | --- |
| No project license or contributor grant | Downloader lacks a LICENSE; ADR-009 unresolved | Owner chooses ownership/distribution/contribution policy; do not treat this doc as permission |
| Apache attribution | Both selected roots have Apache-2.0 LICENSE | Preserve copyright/NOTICE and review nested libraries/assets and license-scanner output |
| Kotlin/desktop third-party obligations | Compose, OkHttp, Arrow, HLS parser, JNA, Android tooling, native packager in Gradle scripts | Dependency SBOM, notices, per-binary attribution, platform build conditions |
| Browser supply chain | npm lockfile with Vite 4, TypeScript 5.9.3, webext-bridge 6.0.1, type-fest dependency | Fix typecheck, review vulnerable/obsolete packages, SBOM and minimal permissions |
| Native/HTTP trust | Native host lists allowed extension IDs; localhost auth disabled by default; silent add possible | Threat model, authenticated pairing and origin rejection tests |
| FFmpeg | LICENSE.md LGPL baseline and GPL components when enabled; vendor builds differ | Approve exact binary/configuration and notices, avoid unreviewed codecs |
| MediaGrabber native binaries | THIRD_PARTY_NOTICES.md identifies a GPLv3 FFmpeg Windows bundle | Avoid accidental incompatible distribution from selective code import |
| GPL/AGPL alternatives | Gopeed, aria2, Cobalt, MeTube, Pinchflat, Tube Archivist, archived UI | Reference only unless explicit owner/legal approval and obligations fulfilled |
| Web URL handling | ReClip accepts arbitrary URL passed to yt-dlp; in-memory jobs; no visible quotas/auth | Do not deploy as a public fetch service; explicit SSRF/redirect/limit design |
| Platform policies | Chrome Web Store specifically flags extensions facilitating YouTube downloads | Test with authorized direct files, complete store policy review before distribution |

## Confidence and next evidence needed

- **High confidence in integration direction:** two primary roots are Apache-2.0 and communicate through a designed native/HTTP bridge; extension bundling is reproducible.
- **Moderate confidence in feature reuse:** source code for engine/DB/queue/HLS is present, but no end-to-end desktop tests or Windows verification performed in this audit.
- **Low confidence in release readiness:** Java 25 toolchain and Gradle bootstrap, TypeScript typecheck, transitive license/SBOM, browser permissions, localhost API authentication and store policy gates remain open.
- **No verified performance ranking:** none of 17 was speed-benchmarked against the same authorized server/hardware.

**Final M0 recommendation to owner:** adopt **AB Download Manager + AB Browser Integration** only after approving license, scope, branding and Windows/Chrome target. Keep one Kotlin engine, its natural companion extension, and add only evidence-backed adapters. Do not import ReClip, MediaGrabber, Gopeed, Motrix or any GPL/AGPL project as a second core.

<!-- END docs/REUSE_DECISION_MATRIX.md -->


---

# SOURCE 18: CONTRIBUTING.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/CONTRIBUTING.md  
Blob: `a12c7684955eab6b61ee1d3f06b1c8b177f811be`

<!-- BEGIN CONTRIBUTING.md -->

# Contributing to Downloader

Thanks for helping create a reliable, privacy-conscious download manager. The project is currently **documentation-first**: architecture and candidate repositories exist, but application code has not yet been imported or validated.

## Before your first contribution

1. Read [the quick start](docs/START_HERE.md), [vision](docs/PRODUCT_VISION.md), [feature matrix](docs/FEATURE_MATRIX.md) and [legal/security requirements](docs/LEGAL_SECURITY.md).
2. Check [open decisions](docs/DECISIONS.md) and the [roadmap](docs/ROADMAP.md).
3. Open or select an issue describing a narrow outcome. Avoid duplicate work and giant PRs.
4. For proposed upstream code, provide the exact repository, commit/tag, license, dependency tree and preservation requirements.
5. Discuss nontrivial changes to architecture, licenses, distribution or network behavior before implementation.

## Current contribution opportunities

- Verify reference-repository licenses, recent activity and dependency security.
- Build a feature coverage matrix from **actual source inspection and test results**.
- Identify usable direct-download fixtures with explicit redistribution/test permission.
- Design the protocol for extension-to-desktop job handoff.
- Improve security and acceptance criteria.
- Propose the initial CI matrix (Linux analysis/build, Windows packaging, browser smoke tests).

After the owner decides the software license and contribution policy, implementation issues can be opened for the first vertical slice.

## Pull request checklist

- [ ] Linked issue and succinct problem statement.
- [ ] Single focused change; unrelated user changes preserved.
- [ ] Clear distinction between implemented, tested and proposed behavior.
- [ ] Tests/documentation updated, commands and results reported.
- [ ] New dependencies justified with license, security and maintenance notes.
- [ ] No secrets, session data, unapproved binaries, media archives or generated downloads.
- [ ] No DRM, paywall, access-control or rights circumvention.
- [ ] License and provenance information supplied for any imported upstream code.

## Engineering conventions (provisional)

- Keep browser, desktop, web and engine modules independent with versioned API contracts.
- Prefer typed interfaces, explicit errors, dependency injection and deterministic tests.
- Handle cancellation, retry, resume and crash recovery as first-class state transitions.
- Include an end-user recovery message with each operational failure.
- Do not add a dependency solely to avoid writing a few lines of simple code; conversely, do not rewrite mature infrastructure.
- Use existing formatting and testing scripts once a chosen base repository is imported.

## Licensing and ownership

**Project license and contribution terms are not yet selected.** Do not assume public visibility means permissive licensing. Contributors retain rights unless an applicable license/contribution agreement says otherwise. Open issues and documentation suggestions are welcome; contact the maintainer before submitting substantial application code or copying third-party components.

## Vulnerability reporting

Do not publish exploit details, credentials or sensitive reproduction data in public issues. Use GitHub's private vulnerability reporting/security advisory mechanism if enabled, or contact the repository owner privately first.

## Proposed PR template

See [.github/PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md). Be factual: never mark a feature ready because it works only in an upstream repository.

<!-- END CONTRIBUTING.md -->


---

# SOURCE 19: SECURITY.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/SECURITY.md  
Blob: `9ff47990372acf60853764b2205d23cb0c53db58`

<!-- BEGIN SECURITY.md -->

# Security policy

This repository is currently a **planning/documentation project**, with no published application build. Security-sensitive issues may still arise in future implementation work or dependencies.

## Reporting a vulnerability

Do **not** publish proof-of-concept exploits, private tokens, user files or personal information in a public issue.

Prefer GitHub private vulnerability reporting or a GitHub Security Advisory if the owner enables it. Otherwise, contact the repository owner privately through their GitHub profile and request a secure reporting channel. The project has no designated security email or response-time SLA yet.

## Engineering policy

All contributors should read [legal and security guardrails](docs/LEGAL_SECURITY.md). In particular, local desktop bridges, native messaging, untrusted URLs, redirect handling, FFmpeg inputs, browser permissions and supply-chain dependencies require careful review.

Only report tested security behavior; no features have been security-audited in this repository at its documentation bootstrap.

<!-- END SECURITY.md -->


---

# SOURCE 20: tools/fixtures/README.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/tools/fixtures/README.md  
Blob: `7688590a996dd1ff46a0d545f82e39c1b821a922`

<!-- BEGIN tools/fixtures/README.md -->

# Deterministic direct-download fixture (M0/M1)

The server is **our original implementation**, independent of candidate upstream applications. It provides a small HTTP test target for the Downloader desktop engine and browser handoff. It never fetches outside URLs, accesses local files, or processes real user videos.

## Start

From repository root, with Python 3.10+:

~~~sh
python3 -m tools.fixtures.http_fixture --port 8765
~~~

The server binds **only to 127.0.0.1**. A port of `0` assigns an available loopback port. Run tests:

~~~sh
python3 -m unittest discover -s tests -v
~~~

The test suite generates a temporary self-signed certificate with the local
OpenSSL CLI and tests real HTTPS with certificate verification **enabled**.
The temporary certificate and key are removed when tests finish.
For a manual HTTPS run, supply your own local test certificate and key:

~~~sh
python3 -m tools.fixtures.http_fixture --port 8765 \
    --tls-cert /path/to/test-cert.pem --tls-key /path/to/test-key.pem
~~~

The HTTPS client must trust that specific test certificate. Never disable
TLS verification globally to make a test pass.

| URL | Behavior | Expected |
| --- | --- | --- |
| `/file.bin` | Fixed 16 KiB binary payload | HTTP 200, `Accept-Ranges: bytes`, deterministic SHA-256 |
| `/file.bin` with `Range: bytes=0-8191` | First half | HTTP 206 + correct `Content-Range` |
| `/file.bin` with `Range: bytes=8192-` | Second half | HTTP 206; concatenate pieces to obtain exact payload hash |
| `/file.bin` with `Range: bytes=99999-` | Invalid offset | HTTP 416 + `Content-Range: bytes */16384` |
| `/no-range.bin` | Deliberately ignores Range | HTTP 200, full body; a downloader must restart safely |
| `/health` | Readiness | HTTP 200 `ok` |
| Any unknown URL | No external proxy | HTTP 404 |

Additional controlled test endpoints and behaviors:

- /file.bin with a matching If-Range ETag: 206 partial resume.
- /file.bin with a stale If-Range ETag: 200 full response (restart).
- /drop.bin: connection closes after 2048 bytes of a declared 16 KiB response; the client must report a truncated transfer.
- /redirect.bin: 302 with a local Location header pointing to /file.bin.

When M1 imports the approved engine, use these endpoints for direct, interrupted/resumed, invalid-range, redirect, TLS and integrity tests. The fixture supports only single byte ranges and local redirects. It does not model authenticated remote sources, bandwidth throttling, HLS/DASH, multirange replies or full crash/restart behavior.

CI is defined in .github/workflows/fixture-tests.yml. It runs for relevant pull requests or on manual invocation, with no GPU or hosted downloader deployment. Passing these tests does not prove the future desktop app works.

<!-- END tools/fixtures/README.md -->


---

# SOURCE 21: .github/workflows/fixture-tests.yml

Original: https://github.com/kawakaze430/Downloader-/blob/main/.github/workflows/fixture-tests.yml  
Blob: `e453e2ff53278d09220f4dc296760546a3481663`

<!-- BEGIN .github/workflows/fixture-tests.yml -->

name: Deterministic download-fixture tests

on:
  pull_request:
    paths:
      - ".github/workflows/fixture-tests.yml"
      - "tests/**"
      - "tools/fixtures/**"
  workflow_dispatch:

permissions:
  contents: read

jobs:
  fixture:
    runs-on: ubuntu-latest
    timeout-minutes: 5
    steps:
      - name: Check out source
        uses: actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4
      - name: Set up Python
        uses: actions/setup-python@a26af69be951a213d495a4c3e4e4022e16d87065 # v5
        with:
          python-version: "3.12"
      - name: Verify local TLS fixture prerequisite
        run: openssl version
      - name: Test deterministic HTTP and HTTPS fixtures
        run: python -m unittest discover -s tests -v

<!-- END .github/workflows/fixture-tests.yml -->


---

# SOURCE 22: docs/context-archive/CHAT_HISTORY.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/context-archive/CHAT_HISTORY.md  
Blob: `c2309563c1159ebc484ed1803595ae591ba43b10`

<!-- BEGIN docs/context-archive/CHAT_HISTORY.md -->

# Downloader — reconstructed conversation history and decision trail

**Archive prepared:** 2026-11-16  
**Canonical repository at archive time:** https://github.com/kawakaze430/Downloader-  
**Earlier repository identity:** https://github.com/rejoansardar620-lang/Downloader-  
**Evidence standard:** This is a faithful *structured account of the user-visible conversation*, not a byte-for-byte ChatGPT export. It preserves the substantive requests, recommendations, corrections, links, decisions, tests and caveats available in the current chat. Private internal reasoning, inaccessible tool payloads and the unreadable attached TXT are not reproduced. For an exact verbatim transcript, export the chat separately from ChatGPT and store a reviewed/redacted copy.

## Quick reverse chronology (newest first)

| Stage | User's objective | Outcome / status |
| --- | --- | --- |
| 12 | Preserve the entire project chat/research in GitHub for continuity | This archive, master consolidated documentation, a Word-readable copy and a resume prompt |
| 11 | Check write access to kawakaze430/Downloader- | Confirmed connected GitHub account has push/admin permission; no files changed by that check |
| 10 | Execute the master engineering prompt and start M0 | M0 source study and build probes; 14/14 fixture tests; commit 042671dc; main desktop app not yet imported |
| 9 | Check PRs and publish all study repositories | At that check, no open PRs; created 17-project STUDY_REPOSITORIES.txt; commit 3509d51 |
| 8 | Start implementation in the right order | Ordered checklist, M0 audit, HTTP fixture, GitHub issues #4, #5, #6 |
| 7 | Make project understandable to contributors | Initial docs/README/AGENTS/issues; commit 51022e8 |
| 6 | Avoid burning Codex credits rebuilding available code | Discovered AB Download Manager + AB Browser Integration as preferred reuse baseline |
| 5 | Compete with top market tools | Benchmarks: IDM, 4K Video Downloader Plus, Video DownloadHelper, Cobalt, JDownloader |
| 4 | Single repository or combined upstreams | Study multiple repositories, but deliver ONE original maintainable product repository |
| 3 | Own and brand the software; use Codex Cloud | MIT-compatible initial options discussed, with ownership/licensing distinctions |
| 2 | Find public open-source downloader repos | Website and extension candidates including MeTube, ReClip and MediaGrabber |
| 1 | Build a YouTube-downloader website/extension | Initial architecture and policy/rights limitations discussed |

## Detailed user–assistant history, oldest to newest

### 1. Initial request: downloader website or extension

**User:** “I would like to create youtube video downloader extension/website can u pls guide”

**Discussion/outcome:** Initial exploration of Next.js/TypeScript frontend, FastAPI or equivalent backend, optional FFmpeg, browser Manifest V3 and a shared architecture. Features discussed included URL input, quality selection, audio/video conversion, queues, progress, playlists and batching. These were prospective features—not implemented ones. YouTube terms and Chrome Web Store restrictions were flagged; production scope must be authorized media.

### 2. Existing public repositories

**User:** “I want to find any public repo available that type of”

**Discussion/outcome:** Examined website/extension examples: MeTube, ReClip, Pinchflat, YouTubeDL-Material, Tube Archivist, yt-dlp-web-ui; Super Video Downloader, MediaGrabber, HLS Downloader, Tube Archivist Companion, Media Grabber. Repositories were presented as study references and possible starting points, not an approved code merger. The final consolidated study list was narrowed/expanded into 17 project references; see docs/STUDY_REPOSITORIES.txt.

### 3. Software belonging to the project owner, and Codex Cloud

**User:** “i want to create my own software system which is belong to me which repo should i consider and then gpt astra codex will code and build for my own and can astra in codex cloud build”

**Discussion/outcome:** Initial recommendation emphasized ReClip and MediaGrabber because their reported MIT licenses are comparatively permissive. Explained that owning the new project's branding, infrastructure and original code does **not** transfer copyright ownership of upstream components. Discussed Codex Cloud as a coding/testing aid, not a guarantee of production readiness, platform approval, or deployment. No release or ownership/legal approval was made.

### 4. Single repo versus combination

**User:** “is combination repo or single repo should i take if i want to build every feature”

**Discussion/outcome:** Recommended reviewing multiple upstreams, integrating only compatible components, and publishing a **single well-structured product monorepo**. Initial options mentioned ReClip (web), MediaGrabber (extension), HLS Downloader (non-DRM media), FFmpeg, yt-dlp (rights-reviewed), and MeTube as an AGPL reference. Important correction later: this was an exploratory combination; do not blindly merge.

### 5. Ambition: a top competitor

**User:** Wanted to create software competitive with the best available download managers—not a simple single-site link downloader.

**Discussion/outcome:** Benchmarked 4K Video Downloader Plus (media workflows), Internet Download Manager (recovery/acceleration), Video DownloadHelper (browser media detection), Cobalt (web UX), and JDownloader 2 (queues/automation). These are **feature benchmarks**, not evidence of an objective market #1 ranking. Product differentiated by reliability, privacy, local-first operation, strong UX, organization, security and quality engineering. Early speculative Tauri/Next.js/FastAPI architecture was later superseded by the recommendation to retain mature Kotlin/Compose upstream desktop internals.

### 6. Avoid unnecessary Codex spending

**User:** “or anything would u like to say which is publicall available in market but codex is building and wasteing my credit so we can include them right now. to achive our main goal”

**Discussion/outcome:** Identified [AB Download Manager](https://github.com/amir1376/ab-download-manager) and its [browser companion](https://github.com/amir1376/ab-download-manager-browser-integration) as stronger starting candidates for a desktop-first system. Added Gopeed, Motrix and aria2 to research comparisons. Recommended feature coverage analysis before coding: REUSE / ADAPT / BUILD with licenses, tests and verification. Core rule: do not ask Codex to rebuild mature functionality without demonstrating an actual gap. AB desktop and browser extension eventually became **exactly two provisional primary foundations**.

### 7. Original GitHub documentation push

**User:** “pls push all in this repo https://github.com/rejoansardar620-lang/Downloader- in that way any developer see and understand everythign about our project goal can start countributing”

**Discussion/outcome:** Initial contributor documentation published to the original repository URL. Reported initial commit [51022e8](https://github.com/kawakaze430/Downloader-/commit/51022e8f6440c3dabba46179c5642a7c8c3d3a3e), including README, AGENTS, product vision, architecture, legal/security, roadmap and contributing guidance. GitHub issues #1–#3 were created for upstream audit, licenses, and a secure download test. These are historical statements; check the current repo for current issue status.

### 8. Begin development, ordered checklist

**User:** “pls start implementing this project if anything block i will give codex to fix it first produce complete checklist in order implement this project”

**Discussion/outcome:** Created ordered checklist (G, M0–M6) with release gates and evidence statuses. Probed AB upstream extension builds and desktop Gradle environment. Found (a) Gradle 9.8 bootstrap blocked in test environment, (b) extension standalone TypeScript validation failures despite successful bundling, and (c) insecure-by-default optional localhost API authentication in upstream integration. Created issues [#4](https://github.com/kawakaze430/Downloader-/issues/4), [#5](https://github.com/kawakaze430/Downloader-/issues/5), [#6](https://github.com/kawakaze430/Downloader-/issues/6). Built original Python local deterministic HTTP fixture and tests. These findings require new verification before claiming fixed.

### 9. Pull requests, 17-repository inventory

**User:** “Can you check? Is there any pull requests is open? Can you merge them? And also, what are the previous repositories you suggested for study for everything? Can you please list them and produce a .txt file and list all the them, and push on current repo”

**Discussion/outcome:** Reported zero open pull requests **at the time of that check**. Created docs/STUDY_REPOSITORIES.txt inventory of 17 upstream repositories and updated README. Reported commit [3509d51](https://github.com/kawakaze430/Downloader-/commit/3509d5158630416b656508e2fe35a57265148752). Do not infer current PR state from this historical observation.

### 10. User's MASTER ENGINEERING PROMPT and M0 execution

**User:** Gave an extensive principal-engineer directive. Essential conditions:

- Read full current repository, docs and tests before changes.
- Deeply investigate all 17 references and choose **exactly two primary foundations**.
- Provisional pair: AB desktop pinned to `68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68`; AB browser integration pinned to `bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05`.
- Reproduce desktop Gradle, browser TypeScript, security and dependency issues; do not fake fixes.
- Build desktop-first/local-first; prefer reusing Kotlin/Compose AB instead of rewriting as Tauri.
- Do not import substantial third-party code before owner licensing/architecture approval.
- Proceed G → M0 foundation audit → M1 direct HTTPS downloader → M2 secure Chrome extension → M3 media → M4 website → M5 releases → M6 advanced capabilities.
- Security: native messaging pairing; localhost origin/auth/CSRF/SSRF; redirects and private-network blocking; safe paths; no DRM circumvention.
- Run deterministic fixture tests; preserve licenses; no paid services or deployment without permission.
- Accuracy priority: correctness > security > reliability > reuse > maintainability > performance > polish.

**Discussion/outcome:** Published M0 deeper reports and fixture enhancements in [042671dc](https://github.com/kawakaze430/Downloader-/commit/042671dc72c8a9b3032e19419d550bec6f518342). Tests grew from **9 to 14** in that run; Python compilation and local git checks passed. Chrome and Firefox upstream extension bundles passed, standalone TS check failed; Gradle/JDK blocked; two high-severity dependency advisories were noted from that dependency audit; secure localhost integration unresolved. Added docs/BUILD_START_PLAN.md and docs/REUSE_DECISION_MATRIX.md. **No integrated desktop app, browser-to-desktop E2E demo, or public release was created.**

### 11. Repository identity / write access

**User:** “can u check in this repo do u have write access? https://github.com/kawakaze430/Downloader-”

**Discussion/outcome:** GitHub connector returned `push=true`, `admin=true`, `maintain=true` for `kawakaze430/Downloader-`, a public repository with `main` default branch. No modifications were made by that permission check. Earlier work and historical links referred to `rejoansardar620-lang/Downloader-`; the current canonical write target is `kawakaze430/Downloader-`. Existing original documents may retain historical URLs, and should not be silently interpreted as a different product.

### 12. Current archival/handoff request

**User:** Asked to collect all charts, messages, research, URLs and documentation into a document in the current GitHub repository so future ChatGPT sessions or contributors can restart reliably.

**Discussion/outcome:** Created the consolidated context package, linked from README. Treat this archive as a historical snapshot—not a substitute for reading current code and CI results.

## Technical decisions retained across the conversation

| Topic | Final working direction | Status |
| --- | --- | --- |
| Business ambition | Independently branded competitor on reliability, privacy, UX | Vision, not ranking |
| Codebase | One first-party product repository, selective upstream reuse | Recommended |
| Primary #1 | AB Download Manager (Kotlin/Compose desktop) | Strong recommendation; owner approval needed |
| Primary #2 | AB Browser Integration (Chrome MV3; Firefox later) | Strong recommendation; owner approval needed |
| Other 15 | Study/reference/optional later components | No automatic import |
| Initial release | Windows 11 + Chrome, authorized direct files | Recommended; owner confirmation pending |
| Media conversion | Local FFmpeg later | Planned, not implemented |
| Website | Later milestone, not initial core | Planned |
| YouTube | Respect restrictions; no promise of unrestricted downloads | Policy constraint |
| Authentication | Prefer native messaging; harden any HTTP bridge | Release gate |
| Code license | No final license selected for original project | Owner decision |
| AI tools | Codex as engineering assistant with tests/review | Not a guarantee of completion |
| Test baseline | Deterministic Python HTTP/HTTPS fixture | Actual project code |
| Completeness | M0 research/testing; desktop and extension not imported | Verified historical state |

## Technical findings to preserve

**AB desktop:** Downstream source investigation described Kotlin/Compose architecture, persistent job/queue models, HTTP download handling, local integration, and native messaging host. Never infer all of these are proven working in our product—they belong to the upstream candidate. Gradle wrapper 9.8 and JDK 25 requirements deserve controlled build verification.

**AB extension:** Pinned source's Chrome and Firefox bundles were built successfully. Standalone TypeScript validation failed with missing native-messaging import and manifest/dependency typings. Chrome packaging in upstream is not the same as a working handoff in our product.

**Integration security:** Some upstream localhost HTTP paths can run with authentication disabled by default. Treat localhost as untrusted from webpage contexts; secure auth, origin/extension identity, CSRF, SSRF, redirects, paths, cookies, token storage and testing are required. Native messaging is preferred where supported.

**Dependency review:** Previous test reported two high-severity findings in an upstream extension dependency tree. Re-run in the approved fork and determine exploitability and remediation before release.

**Original fixture:** `tools/fixtures/http_fixture.py` provides deterministic local HTTP behavior, with Range/no-Range/interrupt/redirect/HTTPS cases; `tests/test_http_fixture.py` reached 14/14 passes in the earlier run. Verify current CI rather than trusting this snapshot alone.

## Links and artifacts

**Canonical GitHub:** https://github.com/kawakaze430/Downloader-  
**Historical GitHub:** https://github.com/rejoansardar620-lang/Downloader-  
**Project start:** docs/START_HERE.md  
**Agent instructions:** AGENTS.md  
**17 repositories:** docs/STUDY_REPOSITORIES.txt  
**Reuse decision matrix:** docs/REUSE_DECISION_MATRIX.md  
**Build plan:** docs/BUILD_START_PLAN.md  
**Implementation checklist:** docs/IMPLEMENTATION_CHECKLIST.md  
**Source audit:** docs/M0_AUDIT.md  
**Open owner decisions:** docs/DECISIONS.md  
**Legal/security constraints:** docs/LEGAL_SECURITY.md  
**Conversation continuity prompt:** docs/context-archive/RESUME_PROMPT.md  
**Consolidated document:** docs/context-archive/PROJECT_MASTER_CONTEXT.md

## Not included / limitations

- Not a literal, independently exported ChatGPT message-by-message transcript. It preserves messages as faithful brief quotations or summaries and decisions.
- Previous tool outputs marked unavailable/redacted in the conversation are not recreated.
- One earlier upload named `Create Video Downloader Extension.txt` was detected, but the file reader returned no readable text. No content was invented from it.
- Past star counts, licenses, releases, vulnerabilities, PR counts, and CI results are dated; refresh live upstream evidence before making release choices.
- No secrets, credentials, private internal reasoning, or paid-service access details are included.

<!-- END docs/context-archive/CHAT_HISTORY.md -->


---

# SOURCE 23: docs/context-archive/RESUME_PROMPT.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/context-archive/RESUME_PROMPT.md  
Blob: `626b17e3a229719333d1c4bb8843c7a14574267b`

<!-- BEGIN docs/context-archive/RESUME_PROMPT.md -->

# START A NEW CHAT OR CODEX TASK HERE

**Canonical project:** https://github.com/kawakaze430/Downloader-

I am continuing the independently branded Downloader project. Do not assume previous chat memory exists.

**Read first, in order:**
1. `docs/context-archive/PROJECT_MASTER_CONTEXT.md` — consolidated historical snapshot, research, diagrams/tables and source docs.
2. `docs/context-archive/CHAT_HISTORY.md` — reconstructed user/assistant decisions from the original long conversation.
3. `AGENTS.md`, `docs/LEGAL_SECURITY.md`, `docs/DECISIONS.md`.
4. `docs/IMPLEMENTATION_CHECKLIST.md`, `docs/M0_AUDIT.md`, `docs/BUILD_START_PLAN.md`.
5. `docs/REUSE_DECISION_MATRIX.md`, `docs/STUDY_REPOSITORIES.txt`.

**Goal:** A commercial-quality local-first desktop download manager, secure Chrome companion, later Firefox, authorized media processing and website. Benchmark IDM, 4K Video Downloader Plus, Video DownloadHelper, Cobalt and JDownloader for capabilities; do not claim market leadership.

**Exactly two primary upstream candidates (NOT imported without approval):**
- https://github.com/amir1376/ab-download-manager — pin `68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68`.
- https://github.com/amir1376/ab-download-manager-browser-integration — pin `bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05`.

**Current historical project status:** M0 research and deterministic HTTP/HTTPS fixtures; 14 tests reported passing in prior environment. No integrated downloader/extension yet. Known issues: Gradle/JDK bootstrap (#4), TypeScript (#5), authenticated companion pairing (#6), third-party license/dependency audit. Owner has not chosen project license nor explicitly approved upstream import.

**Before any action:** Inspect CURRENT GitHub branch, commits, PRs, issue states, code, tests, installed dependencies and CI. Historical archived facts are not live evidence. Keep existing working fixtures. Avoid costly rewrites. Do not bypass DRM or platform restrictions.

**Next safe actions:** verify current CI and issue status; complete M0 dependency/licensing and reproducible desktop build checks; prepare a narrowly scoped PR; request owner approval before importing upstream application code or shipping distributions.

**Response format:** What exists / what was verified today / proposed next change / exact tests / blockers / license or security implications. Never claim tests pass without running them.

<!-- END docs/context-archive/RESUME_PROMPT.md -->
