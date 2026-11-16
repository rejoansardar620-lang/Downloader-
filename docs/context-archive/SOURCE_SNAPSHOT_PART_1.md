# SOURCE DOCUMENT SNAPSHOT — Part 1 of 3

Dated: 2026-11-16. Original documents reproduced from GitHub. These are immutable historical snapshots, not live current-state claims.

---

# SOURCE 1: README.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/README.md  
Blob: `1b10997770d90d695a2f1f9eb5c141ab65f3ad20`

<!-- BEGIN README.md -->

# Downloader — product blueprint

> **Status: planning and M0 engineering audit in progress. The upstream browser-extension candidate builds, but no Downloader application has been imported, implemented or released here yet.**

This is the public collaboration hub for an independently branded, cross-platform **download manager and authorized media-processing platform**. The ambition is to build a credible competitor to established desktop download managers, video downloaders, and browser media tools—not merely a page that accepts a YouTube URL.

The product name, software license, commercial model, and final architecture have **not** been approved yet. "Downloader" is a working name. The project owner controls this repository; contributors retain any rights not transferred or licensed under an agreed contribution policy.

## North star

Create one cohesive experience across:

1. **Desktop** — the flagship application for reliable local downloads, pause/resume, queues, scheduling, file organization, and media conversion.
2. **Browser extension** — discover permitted downloads and hand jobs securely to the desktop application.
3. **Website** — introduce the product and, in a later milestone, offer a secure browser-based interface for user-owned or explicitly downloadable media.
4. **Shared engine and optional services** — reusable jobs, format discovery, media processing, authentication where needed, and diagnostics.

We benchmark **4K Video Downloader Plus** for video workflows, **Internet Download Manager** for download reliability, **Video DownloadHelper** for browser integration, **JDownloader** for automation, and **Cobalt** for web usability. None has been independently established as the single worldwide "#1" across all categories. See [market analysis](docs/COMPETITOR_BENCHMARK.md).

## Project status

| Area | Current status | First next step |
| --- | --- | --- |
| Vision, scope, roadmap | Documented; review requested | Agree on priorities |
| Repository/license audit | 17-repository source triage complete; dependency-level audit open | Unblock desktop build; review permissions, SBOM and NOTICE obligations |
| Desktop application | Not implemented | Evaluate AB Download Manager foundation |
| Browser extension | Not implemented | Evaluate AB Browser Integration foundation |
| Website/download UI | Not implemented | Design original UI; ReClip is a UX reference only pending SSRF/security review |
| Media processing | Not implemented | Verify FFmpeg integration and permitted-source policy |
| Automated tests, CI and releases | 14 local fixture tests pass; minimal PR/manual fixture CI added; no app CI or releases | Verify GitHub workflow and develop product test gates |

Do not interpret the existence of a feature in an upstream project as an implemented feature **here**.

## Start here

- [New contributor quick start](docs/START_HERE.md)
- [Complete ordered implementation checklist](docs/IMPLEMENTATION_CHECKLIST.md) — milestones, dependencies, release gates and blockers
- [M0 engineering audit](docs/M0_AUDIT.md) — source pins, actual Chrome build result, reuse matrix, security findings and handoff
- [Verified 17-repository reuse matrix](docs/REUSE_DECISION_MATRIX.md) — candidates, licenses, exact source commits and decisions
- [Build-start engineering plan](docs/BUILD_START_PLAN.md) — original code inventory, threat model, architecture and M0/M1/M2 gates
- [Local direct-download fixture](tools/fixtures/README.md) — original dependency-free HTTP/optional HTTPS test server; 14 passing tests, no paid network calls
- [Project vision and users](docs/PRODUCT_VISION.md)
- [Complete feature inventory and acceptance criteria](docs/FEATURE_MATRIX.md)
- [Reference repositories, licenses and reuse strategy](docs/REFERENCE_REPOSITORIES.md)
- [Complete study repository list (TXT)](docs/STUDY_REPOSITORIES.txt) — all 17 previously suggested open-source projects, categories and study order
- [Proposed system architecture](docs/ARCHITECTURE.md)
- [Milestones and definition of done](docs/ROADMAP.md)
- [Competitor benchmarks](docs/COMPETITOR_BENCHMARK.md)
- [Legal, copyright, privacy and security guardrails](docs/LEGAL_SECURITY.md)
- [Development and testing setup](docs/DEVELOPMENT_SETUP.md)
- [Codex/cloud workflow and credit-saving instructions](docs/CODEX_WORKFLOW.md)
- [Open architectural decisions](docs/DECISIONS.md)
- [How to contribute](CONTRIBUTING.md)
- [AI contributor instructions](AGENTS.md)

## Recommended starting points (not imported)

| Purpose | Candidate | Why |
| --- | --- | --- |
| Main desktop starting point | [AB Download Manager](https://github.com/amir1376/ab-download-manager) | Mature open-source download management; Apache-2.0 |
| Browser companion | [AB browser integration](https://github.com/amir1376/ab-download-manager-browser-integration) | Already works with AB application; Apache-2.0 |
| Web-download experience (reference only) | [ReClip](https://github.com/averygan/reclip) | MIT Flask/JS UX patterns; arbitrary remote URL handling is not a secure foundation for a hosted service |
| Media discovery | [MediaGrabber](https://github.com/miroshArtem/MediaGrabber) and [HLS Downloader](https://github.com/puemos/hls-downloader) | Extension architecture and non-DRM streams; both MIT |
| Media operations | [FFmpeg](https://github.com/FFmpeg/FFmpeg) | Conversion/remuxing with build-dependent licenses |
| Optional extraction | [yt-dlp](https://github.com/yt-dlp/yt-dlp) | For sources and uses that permit downloading; check bundled licenses |

**Important:** These are research inputs, not automatically approved dependencies. The software must not facilitate unauthorized access, DRM circumvention, or prohibited copying.

## Working rules

- **Reuse verified, appropriately licensed implementations.** Do not spend time rebuilding existing queues, scheduling, browser handoff, FFmpeg, or installers without a demonstrated need.
- **Keep one product repository** with modular packages rather than pasting unrelated projects together.
- **Implement in verified increments.** Every feature needs tests, meaningful errors and a clear status.
- **Privacy and safety first.** Protect user files, download permissions, cookies, local helper APIs and cloud services.
- **Do not promise universal website support.** Compatibility changes, permissions and platform policies limit availability.

## Contributing right now

The first milestone is an **evidence-backed repository and license audit**, not an entire product rewrite. Choose an issue, read [CONTRIBUTING.md](CONTRIBUTING.md), and propose improvements via a focused PR. The owner must select a software license and contribution policy before accepting substantial new application code.

## Links and scope

- Project: https://github.com/rejoansardar620-lang/Downloader-
- Research snapshot: **15 November 2026**; upstream capabilities and licenses must be rechecked before adoption.
- This repository does not contain source code from any reference project at this stage.

<!-- END README.md -->


---

# SOURCE 2: AGENTS.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/AGENTS.md  
Blob: `baad5e3d6e5d99fa56103678a1b329d87812fc22`

<!-- BEGIN AGENTS.md -->

# Instructions for coding agents and AI contributors

You are working on **Downloader**, a planned desktop-first download manager, browser companion and authorized media-processing platform. Read this file and the linked product documents before changing code.

## Source of truth, in order

1. [Legal and security requirements](docs/LEGAL_SECURITY.md).
2. [Decisions and unresolved owner choices](docs/DECISIONS.md).
3. [Ordered implementation checklist](docs/IMPLEMENTATION_CHECKLIST.md), [product vision](docs/PRODUCT_VISION.md) and [feature matrix](docs/FEATURE_MATRIX.md).
4. [Architecture](docs/ARCHITECTURE.md) and [roadmap](docs/ROADMAP.md).
5. Existing source files, tests and upstream documentation once code is imported.

If sources disagree, flag the conflict and request a decision. Do not invent requirements or treat proposed architecture as implemented.

## Mandatory workflow

1. **Inspect before editing.** Check branch, worktree, related docs, existing implementations, dependency licenses and tests. Preserve unrelated changes.
2. **Classify the request** as investigation, audit, documentation, implementation, testing or deployment.
3. **Reuse before building.** For each proposed feature, state whether it already exists, needs adaptation, or is genuinely missing. Check license/security compatibility.
4. **Make a narrow plan** with acceptance criteria and expected tests. Do not attempt the whole roadmap in one task.
5. **Implement only authorized scope**, using clear interfaces and least-privilege access.
6. **Verify** with relevant tests/builds and a real, permitted fixture. Separate observed results from assumptions.
7. **Report** files changed, tests run, remaining blockers and updated feature status.

## Cost control for Codex Cloud

- First task: **audit and gap matrix only**. No bulk code import or large rewrite without owner approval.
- Prioritize the smallest end-to-end vertical slice, then expand.
- Do not regenerate working code merely to change framework or style.
- Cache dependencies in CI, use targeted tests and avoid expensive video downloads in routine runs.
- Prefer small deterministic test media and mock adapters; run real-network compatibility tests separately and with permission.
- Do not provision cloud servers, enable paid services or deploy publicly without explicit owner approval.
- Commit meaningful progress frequently; never rely on disposable task storage for critical work.

## Safety and correctness

- Do not circumvent DRM, paywalls, authentication, geo-controls, or copyright restrictions.
- Respect YouTube and other platform policies; official APIs do not generally authorize raw media downloading.
- Public Chrome extension releases must comply with Chrome Web Store policies (including YouTube restrictions).
- Never log or commit cookies, passwords, tokens, private URLs, user media, secrets or personal identifiers.
- Validate remote URLs and redirects; block private network destinations and metadata endpoints; limit resource usage.
- Protect local companion APIs against arbitrary websites and cross-origin abuse; use pairing, origin checks and authentication.
- Sandbox media parsing/transcoding and use trusted, verified dependencies.
- Preserve original copyright and license notices for reused code; use the approved project license only after owner decision.

## Project maturity

As of the first [M0 audit](docs/M0_AUDIT.md) there is **no application implementation** here. The pinned upstream Chrome extension builds, but no product runtime was imported or tested end to end. A documented upstream feature is **not** an implemented local feature. Do not mark any item Complete until the defined evidence is recorded.

## Suggested first deliverables

1. Upstream audit including license/SPDX/NOTICE, dependency health and tested builds.
2. Decision record for base application and shared engine.
3. Minimal app + extension integration with a user-authorized direct-download test fixture.
4. Automated tests, packaging and documentation for that slice.

Follow [CONTRIBUTING.md](CONTRIBUTING.md) for human review and PR expectations.

<!-- END AGENTS.md -->


---

# SOURCE 3: docs/START_HERE.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/START_HERE.md  
Blob: `290d18988079c51f17b5aa23fa3620c35c3d7a5a`

<!-- BEGIN docs/START_HERE.md -->

# Start here: new developer onboarding

**Current status:** The repository contains a plan and contributor instructions, not a running downloader.

## Understand the product in 10 minutes

1. [README](../README.md) — overview and current implementation status.
2. [Product vision](PRODUCT_VISION.md) — users, competition and product boundaries.
3. [Feature matrix](FEATURE_MATRIX.md) — exact capabilities, priority and acceptance.
4. [Ordered implementation checklist](IMPLEMENTATION_CHECKLIST.md) — exact task order and stage gates.
5. [Architecture](ARCHITECTURE.md) — intended components and interaction.
6. [Roadmap](ROADMAP.md) — what to build first.
7. [Reference projects](REFERENCE_REPOSITORIES.md) — what to evaluate and reuse.
8. [Legal/security](LEGAL_SECURITY.md) — non-negotiable safeguards.
9. [Codex workflow](CODEX_WORKFLOW.md) — budget-conscious AI implementation.
10. [Verified 17-source research matrix](REUSE_DECISION_MATRIX.md) — exactly two recommended foundations; all commit pins and licensing warnings.
11. [Build-start and security execution plan](BUILD_START_PLAN.md) — source inventory, M0 blockers, staged M1/M2 acceptance gates.

## First contribution: repository audit

**Expected output, before any large import:**

- Pin a commit/tag for each proposed source project.
- Record language, frameworks, build instructions, release activity and tests.
- Verify licenses including transitive libraries, bundled tools and NOTICE obligations.
- Build and test the proposed base in an isolated environment; capture real results.
- Map each feature to **implemented upstream / partially implemented / missing**, and separately to **implemented here / unimplemented**.
- Compare integration costs, security risks, package sizes and maintenance burden.
- Recommend one base app and extension with a small working integration target.

Use the issue template. Put audit results in a PR; avoid copying upstream trees before approval.

**Current M0 evidence (15 November 2026):** The 17-source triage and two-foundation recommendation have been documented; the original Python HTTP/HTTPS fixture has **14 passing local tests** and a small fixture-only CI workflow. The upstream Chrome/Firefox bundles compile but fail independent TypeScript checks; desktop Gradle bootstrap is network-blocked. See [M0_AUDIT.md](M0_AUDIT.md). These results do not constitute a working downloader.

## Owner decisions needed before substantial code imports

1. Product/brand name and trademark checks.
2. Intended distribution: open-source, source-available, or proprietary.
3. Project license and contributor terms (no LICENSE has been chosen).
4. Initial supported OS/browser pair (recommend Windows + Chrome, then Linux/Firefox).
5. Whether the initial web app supports uploads, authorized direct URLs, or only marketing.
6. Whether any third-party source requires written authorization before downloads.

## Suggested first runnable demonstration

With permission to implement, show a Chrome extension sending an **authorized HTTPS file URL** to a desktop app. The app downloads with progress, pause/resume where the server supports range requests, checksum verification, and restart recovery. A short test fixture proves the flow. This validates the hardest shared integration without relying on YouTube compatibility.

## Common mistakes to avoid

- Calling a planned feature "implemented."
- Importing AGPL/GPL code into a closed-source product without resolving obligations.
- Copying user credentials into test fixtures or CI.
- Building the whole stack before validating one application-extension bridge.
- Depending on scraping or external network behavior for the entire test suite.

<!-- END docs/START_HERE.md -->


---

# SOURCE 4: docs/STUDY_REPOSITORIES.txt

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/STUDY_REPOSITORIES.txt  
Blob: `3a5aa76a011848b197fcc082e917c3c1322b87c1`

<!-- BEGIN docs/STUDY_REPOSITORIES.txt -->

DOWNLOADER PROJECT - COMPLETE OPEN-SOURCE STUDY REPOSITORY LIST
================================================================
Research inventory originally prepared: 13 November 2026
List compiled for contributors: 15 November 2026
Project: https://github.com/rejoansardar620-lang/Downloader-
Source of this list: docs/REFERENCE_REPOSITORIES.md and docs/M0_AUDIT.md

PURPOSE
-------
Build our independently branded download manager with:
(1) Desktop app, (2) Chrome/Firefox extension, (3) website,
(4) authorized media tools, (5) optional advanced automation.
Study and reuse existing code only when verified and license-compatible.
This is a STUDY LIST, not a list of projects already imported or approved.
Root licenses below are historical/reported and MUST be rechecked, along
with all transitive dependencies and bundled binaries, before code reuse.

CATEGORY A - PRIMARY REUSE / INTEGRATION CANDIDATES (8)
-------------------------------------------------------

01. AB Download Manager
    URL: https://github.com/amir1376/ab-download-manager
    Reported license: Apache-2.0
    Role: PRIMARY DESKTOP FOUNDATION (provisional).
    Study: Kotlin/Compose app; direct downloads; queues, retries,
      scheduling, persistence, pause/resume, download engine, UI.
    Evidence: Source checked at commit
      68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68.
    Status: Partial M0 audit; desktop Gradle bootstrap blocked by
      environment network access (see GitHub issue #4).
    Do not rewrite: Existing transfer engine until tested and proven missing.

02. AB Download Manager Browser Integration
    URL: https://github.com/amir1376/ab-download-manager-browser-integration
    Reported license: Apache-2.0
    Role: PRIMARY BROWSER EXTENSION FOUNDATION (provisional).
    Study: Chrome MV3, Firefox, native messaging, desktop localhost
      handoff, context menus, download capture, HLS detection.
    Evidence: Source checked at commit
      bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05.
    Status: Chrome and Firefox bundler builds PASS; standalone TypeScript
      check FAIL. See issue #5. Security hardening needed (issue #6).

03. ReClip
    URL: https://github.com/averygan/reclip
    Reported license: MIT
    Role: Possible lightweight website/UI starting point.
    Study: Flask/JavaScript interface, format/quality options, batches,
      user experience, safe server architecture.
    Status: Candidate only; source/dependency/security audit pending.

04. MediaGrabber
    URL: https://github.com/miroshArtem/MediaGrabber
    Reported license: MIT
    Role: Selective extension media-discovery reference.
    Study: Manifest V3, media link discovery, helper communication.
    Status: Audit pending. Existing AB extension already detects HLS;
      do not duplicate its logic without a verified gap.

05. HLS Downloader
    URL: https://github.com/puemos/hls-downloader
    Reported license: MIT
    Role: Selective authorized non-DRM HLS processing reference.
    Study: Stream manifests, local processing, ffmpeg.wasm tradeoffs.
    Status: Audit pending. Do not bypass access controls or DRM.

06. Parabolic
    URL: https://github.com/NickvisionApps/Parabolic
    Reported license: MIT in original inventory (reverify).
    Role: Alternative desktop video/audio workflow candidate.
    Study: Quality/format selection, UX, batch operations,
      Linux/Windows support and dependency costs.
    Status: Compare against AB before selecting a second desktop base.

07. FFmpeg
    URL: https://github.com/FFmpeg/FFmpeg
    Reported license: LGPL or GPL depending on build/configuration.
    Role: Media conversion/remuxing/probing backend.
    Study: MP4/MKV/WebM, audio extraction, subtitles, metadata,
      output validation, resource limits, binary packaging.
    Status: Proposed dependency; specific build flags and licenses unverified.

08. yt-dlp
    URL: https://github.com/yt-dlp/yt-dlp
    Reported license: Core Unlicense; bundled components vary.
    Role: OPTIONAL connector for sources where downloading is permitted.
    Study: Extractor interfaces, metadata, format reporting, update strategy.
    Status: Restricted research candidate, not default public-site support.

CATEGORY B - STUDY FOR DESIGN / FEATURES, NOT AUTOMATIC CODE IMPORT (9)
----------------------------------------------------------------------

09. Gopeed
    URL: https://github.com/GopeedLab/gopeed
    Reported license: GPL-3.0
    Study: Cross-platform engine, modular downloader, extensions,
      API/CLI, automation, design tradeoffs.
    Restriction: GPL license review before any reuse.

10. Motrix
    URL: https://github.com/agalwood/Motrix
    Reported license: Main source MIT; bundled aria2 GPL-licensed.
    Study: Desktop UI, task management, install/update experience,
      aria2 integration, notification and permissions.
    Restriction: Audit entire distribution, not just top-level MIT license.

11. aria2
    URL: https://github.com/aria2/aria2
    Reported license: GPL-2.0
    Study: Segmented downloading, resume, HTTP/FTP/BitTorrent support,
      JSON-RPC, concurrency and error handling.
    Restriction: GPL compatibility required if distributing linked/bundled code.

12. Cobalt
    URL: https://github.com/imputnet/cobalt
    Reported license: AGPL-3.0
    Study: Clean web media UX, API/backend separation, privacy,
      responsive error messages.
    Restriction: Reference only until AGPL/network obligations reviewed.

13. MeTube
    URL: https://github.com/alexta69/metube
    Reported license: AGPL-3.0
    Study: Queue, self-hosted downloads, format choices,
      playlist/channel workflows, task progress.
    Restriction: Reference only until AGPL obligations reviewed.

14. Pinchflat
    URL: https://github.com/kieraneglin/pinchflat
    Reported license: AGPL-3.0
    Study: Recurring scheduled tasks, library names,
      presets, configuration and automation.
    Restriction: Reference only until AGPL obligations reviewed.

15. Tube Archivist
    URL: https://github.com/tubearchivist/tubearchivist
    Reported license: GPL-3.0
    Study: Media library, search, tagging, organization, indexing,
      long-running worker/service design.
    Restriction: GPL compatibility review before any reuse.

16. YoutubeDL-Material
    URL: https://github.com/Tzahi12345/YoutubeDL-Material
    Reported license: MIT
    Study: Full-stack web UI, download queues, user settings,
      multi-user service patterns.
    Restriction: Check maintenance, dependencies and security carefully.

17. yt-dlp-web-ui
    URL: https://github.com/marcopiovanello/yt-dlp-web-ui
    Reported license: GPL-3.0; recorded as archived October 2026.
    Study: RPC and web/worker communication patterns.
    Restriction: Not recommended as primary maintained foundation;
      verify archived status and GPL before any reuse.

COMPETITOR PRODUCTS TO BENCHMARK (NOT REUSABLE SOURCE REPOS)
-----------------------------------------------------------
A. 4K Video Downloader Plus - https://www.4kdownload.com/
   Study: Video workflows, format selection, playlists, subtitles.
B. Internet Download Manager (IDM) - https://www.internetdownloadmanager.com/
   Study: Reliability, browser capture, resume, speed and scheduling.
C. Video DownloadHelper - https://www.downloadhelper.net/
   Study: Extension UX, discoverability, companion architecture,
     browser-store requirements. Notably Chrome store limits YouTube support.
D. JDownloader 2 - https://jdownloader.org/
   Study: LinkGrabber, batch workflows, filters, automation.
E. Cobalt - see #12 above.
   Study: Minimal web interface and API UX.

RECOMMENDED STUDY ORDER
-----------------------
PHASE 1  Read docs/M0_AUDIT.md and docs/IMPLEMENTATION_CHECKLIST.md.
PHASE 2  Complete AB desktop build/tests (01) and extension audits (02).
PHASE 3  Compare the AB stack with Parabolic (06); do NOT merge two
         complete desktop applications without a measured reason.
PHASE 4  Audit media choices (07, 08, 04, 05), security and licenses.
PHASE 5  Evaluate website UI candidate ReClip (03) and related examples
         (12, 13, 16, 17) for feature patterns.
PHASE 6  Review downloader engine approaches (09, 10, 11) and
         media-library/automation patterns (14, 15).
PHASE 7  Make a feature-by-feature BUILD vs REUSE vs ADAPT vs DEFER matrix.
PHASE 8  Obtain owner signoff for brand, license, first platform and
         source policy before importing upstream implementations.

SECURITY, RIGHTS AND LICENSE RULES
----------------------------------
- Do not import all 17 repositories into one codebase.
- Prefer one verified core and one compatible extension.
- Preserve copyright, LICENSE and NOTICE requirements.
- GPL/AGPL code is not silently compatible with a proprietary product.
- FFmpeg licensing depends on actual binaries, codecs and build flags.
- Never copy logos, names, trademarks, proprietary assets or credentials.
- Do not bypass DRM, login, paywalls or site terms.
- Test only with user-owned or explicitly permitted download sources.
- Protect localhost APIs against unauthorized webpages and commands.
- No unsupported promises of universal or YouTube downloading.

CURRENT EVIDENCE / HANDOFF
--------------------------
M0 audit: https://github.com/rejoansardar620-lang/Downloader-/blob/main/docs/M0_AUDIT.md
Checklist: https://github.com/rejoansardar620-lang/Downloader-/blob/main/docs/IMPLEMENTATION_CHECKLIST.md
Original reference matrix:
  https://github.com/rejoansardar620-lang/Downloader-/blob/main/docs/REFERENCE_REPOSITORIES.md
Gradle blocker: https://github.com/rejoansardar620-lang/Downloader-/issues/4
TypeScript blocker: https://github.com/rejoansardar620-lang/Downloader-/issues/5
Security pairing: https://github.com/rejoansardar620-lang/Downloader-/issues/6

Last inventory check: 15 November 2026.
This file documents PRIOR SUGGESTIONS, not a new independent license audit.

<!-- END docs/STUDY_REPOSITORIES.txt -->


---

# SOURCE 5: docs/REFERENCE_REPOSITORIES.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/REFERENCE_REPOSITORIES.md  
Blob: `0faab7708d96a21cdcee3fb5084f9082b1cc0b0c`

<!-- BEGIN docs/REFERENCE_REPOSITORIES.md -->

# Open-source landscape and reuse decisions

Snapshot researched on **13 November 2026**. Links, licenses, activity and features can change. **A complete dependency and provenance audit is required before code reuse or distribution.**

## Primary candidates: evaluate for actual integration

| Project | URL | Reported project license | Main value | Initial decision |
| --- | --- | --- | --- | --- |
| AB Download Manager | https://github.com/amir1376/ab-download-manager | Apache-2.0 | Kotlin Multiplatform/Compose desktop manager; acceleration, queues, scheduling | **Preferred desktop base, pending build/security audit** |
| AB browser integration | https://github.com/amir1376/ab-download-manager-browser-integration | Apache-2.0 | Existing extension/desktop handoff, Chrome/Firefox packaging | **Preferred extension base, pending audit** |
| ReClip | https://github.com/averygan/reclip | MIT | Lightweight Flask web media frontend; MP4/MP3, quality and batch | **Candidate website/UI implementation** |
| MediaGrabber | https://github.com/miroshArtem/MediaGrabber | MIT | Manifest V3 detection and native helper reference | **Evaluate selectively**; smaller project, assess tests/security |
| HLS Downloader | https://github.com/puemos/hls-downloader | MIT | Authorized HLS with local ffmpeg.wasm processing | **Evaluate selectively** |
| Parabolic | https://github.com/NickvisionApps/Parabolic | MIT | Desktop video/audio UX, formats and supported OS workflows | **Compare with AB; do not rebuild desktop twice** |
| FFmpeg | https://github.com/FFmpeg/FFmpeg | LGPL/GPL depending on configuration | Media transcoding, remuxing, codecs, metadata | **Use as dependency after binary/license review** |
| yt-dlp | https://github.com/yt-dlp/yt-dlp | Core Unlicense; bundled components differ | Site extractors, format/metadata handling | **Optional, permitted-source plugin only** |

## References: features and patterns, not automatic imports

| Project | URL | License note | What to study |
| --- | --- | --- | --- |
| Gopeed | https://github.com/GopeedLab/gopeed | GPL-3.0 | Cross-platform download engine, extension model, API, CLI, automation |
| Motrix | https://github.com/agalwood/Motrix | Main code MIT; bundled aria2 is GPL-licensed | Desktop UX, plugins, native messaging, SBOM |
| aria2 | https://github.com/aria2/aria2 | GPL-2.0 | Segmented transfers, resume, protocol support, JSON-RPC |
| Cobalt | https://github.com/imputnet/cobalt | AGPL-3.0 | Web UX and API separation |
| MeTube | https://github.com/alexta69/metube | AGPL-3.0 | Format options, queue, playlist/channel subscriptions |
| Pinchflat | https://github.com/kieraneglin/pinchflat | AGPL-3.0 | Download scheduling, media naming, user-defined automation |
| Tube Archivist | https://github.com/tubearchivist/tubearchivist | GPL-3.0 | Media library, search and organizational workflows |
| YoutubeDL-Material | https://github.com/Tzahi12345/YoutubeDL-Material | MIT | Full-stack UI and downloader service; audit older dependencies |
| yt-dlp-web-ui | https://github.com/marcopiovanello/yt-dlp-web-ui | GPL-3.0; archived October 2026 | RPC patterns only; not recommended as primary maintained base |

## Important distinction: source license versus product ownership

- The owner may control the repository, new original code, brand and deployment **without** acquiring exclusive copyright in upstream code.
- **MIT/Apache-2.0** are permissive but still require copyright notices and/or LICENSE/NOTICE preservation; inspect each dependency.
- **GPL/AGPL** often require source-availability obligations under distribution or network use; study as references unless the owner explicitly approves the relevant licensing model.
- FFmpeg builds vary in applicable license conditions; generated binaries, external codecs and nonfree flags must be reviewed.
- A separately installed program, embedded library, linked component and copied source code may create **different** obligations. Obtain appropriate legal review before commercial distribution.
- Branding and logos are independent of code licenses. Do not reuse third-party trademarks or UI assets as our own.

## Audit checklist and evidence format

For **each** adoption candidate:

1. Pin source URL, commit SHA/tag, commit date and maintainer status.
2. Read project license, CONTRIBUTING, NOTICE, third-party notices and build files.
3. Enumerate transitive dependency licenses, bundled executables and media codecs.
4. Identify known vulnerabilities, unsafe install scripts, open network listeners and insecure defaults.
5. Build from source in a clean environment and record exact command, OS, logs and artifacts.
6. Test relevant features with an authorized sample; verify what works instead of repeating README claims.
7. Estimate maintenance burden, compatibility and expected integration changes.
8. Record **reuse / adapt / reference only / reject**, with decision and reviewer.

Suggested report row:

| Component | Ref/commit | Build | License/SBOM | Verified features | Risks | Decision |
| --- | --- | --- | --- | --- | --- | --- |
| Example | To inspect | Not run | Pending | None verified | Pending | Undecided |

## Research sources

Primary project sources are the GitHub repositories above. Product benchmarks and policies are separately linked in [COMPETITOR_BENCHMARK.md](COMPETITOR_BENCHMARK.md) and [LEGAL_SECURITY.md](LEGAL_SECURITY.md).

<!-- END docs/REFERENCE_REPOSITORIES.md -->


---

# SOURCE 6: docs/PRODUCT_VISION.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/PRODUCT_VISION.md  
Blob: `68a34e995b08c3c487ec500b719add25db9469ec`

<!-- BEGIN docs/PRODUCT_VISION.md -->

# Product vision and strategy

## Mission

Create a leading independent **download management and authorized media-processing** system with the reliability of a professional desktop download manager, the convenience of a browser extension, and the approachable experience of a modern website. "No. 1" is an aspiration, **not an achieved ranking or verified market claim**.

## Users and jobs

| User | Primary job | Product response |
| --- | --- | --- |
| Everyday desktop user | Save large permitted files without restarting after interruptions | Reliable queue, progress, resume, integrity checks |
| Creator/editor | Convert and organize their own or licensed media | Local FFmpeg workflows, formats, metadata, subtitles |
| Power user | Manage many legitimate downloads efficiently | Scheduling, batch intake, history, filtering, CLI/API later |
| Browser-first user | Save downloadable files without switching context | Small, privacy-conscious browser companion |
| Self-hosting user (later) | Manage their own permitted file-processing workflow remotely | Authenticated web UI and optional self-hosted service |

## Competitive benchmarks

- **4K Video Downloader Plus:** approachable desktop UX; presets, high-quality formats, subscriptions/playlists when authorized.
- **Internet Download Manager:** robust queueing, segmented transfers, resume/recovery and browser integration.
- **Video DownloadHelper:** discover permitted media from browser pages and provide understandable conversion choices.
- **JDownloader:** batch intake, automation and download management.
- **Cobalt:** simple web interaction and low-friction flows.

These are **feature inspirations**, not permission to copy trademarks, designs, closed-source code or protected content. See [sources and scope](COMPETITOR_BENCHMARK.md).

## Differentiators to prove

1. **Reliability:** recover cleanly from interruption and system restart.
2. **Local-first privacy:** prefer on-device downloading and media processing; clear disclosures when cloud services are used.
3. **One shared experience:** desktop, extension and website use a coherent job model and design language.
4. **Trust:** no misleading buttons, bundled adware, silent credential collection or opaque third-party uploads.
5. **Good defaults:** safe filenames, sensible format presets, helpful errors, minimal permissions.
6. **Developer-ready extensibility:** source adapters and API contracts can evolve independently.

## Business and quality outcomes (targets, not results)

- First public beta: installable desktop application plus working extension for permitted direct downloads, with reproducible automated tests.
- Later product: responsive web UI, localized design, signed releases, auto-updates and supported conversion workflows.
- Measure: completion rate, interruption recovery, median support resolution, download integrity, install success, crash-free sessions and user retention.
- Do not promise universal site support, guaranteed acceleration multipliers, guaranteed 8K, DRM removal or Chrome Web Store approval.

## Non-goals and boundaries

- No access-control, paywall, DRM, authentication or geo-block circumvention.
- No downloading copyrighted YouTube audiovisual media without YouTube permission/appropriate service authorization.
- No bulk hosting of third-party media as a default; minimize server bandwidth/storage costs.
- No rewriting established open-source infrastructure merely for novelty.
- No permanent product branding, license or pricing decision without owner confirmation.

## Product release ordering

1. **Desktop + browser companion** for ordinary authorized file downloads.
2. **Optional local media processing** of uploaded/owned files, with tested format support.
3. **Website** and authenticated remote features where lawful and operationally sustainable.
4. **Advanced automation and integrations** only after reliability, security and license audits pass.

<!-- END docs/PRODUCT_VISION.md -->


---

# SOURCE 7: docs/FEATURE_MATRIX.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/FEATURE_MATRIX.md  
Blob: `5cfb313e0197fa49c172e0cd3980aa6f9cfc0dff`

<!-- BEGIN docs/FEATURE_MATRIX.md -->

# Master feature inventory and acceptance criteria

This is the feature backlog for the intended competitive product. **All application features listed below remain NOT IMPLEMENTED in this repository as of 15 November 2026.** Separately, the repo includes an original deterministic HTTP/HTTPS fixture with 14 passing tests and a fixture-only CI workflow. Neither makes the app features complete. A named upstream library is a candidate to evaluate, **not proof** that our product implements the feature. See the [17-source evidence matrix](REUSE_DECISION_MATRIX.md) and [M0 execution report](BUILD_START_PLAN.md).

Legend: **P0** required for first beta; **P1** important for a competitive 1.x product; **P2** later differentiation. Acceptance criteria below are proposed and may be refined after audit.

## A. Desktop download manager

| ID | Feature | Priority | Candidate | Acceptance target |
| --- | --- | --- | --- | --- |
| D01 | HTTPS direct-file downloads | P0 | AB Download Manager | Download a permitted test file with matching SHA-256 |
| D02 | Real progress, ETA and speed | P0 | AB Download Manager | UI reflects actual transferred bytes; handles unknown length |
| D03 | Pause/resume | P0 | AB Download Manager | Range-capable source resumes without corrupting output; otherwise safe restart |
| D04 | Retry and recovery | P0 | AB Download Manager | Simulated disconnect/restart preserves recoverable job state |
| D05 | Cancel and cleanup | P0 | AB Download Manager | Job cancels without orphaned worker or unsafe partial files |
| D06 | Download queue and concurrency cap | P0 | AB Download Manager | Configured concurrency and fair scheduling enforced |
| D07 | Safe filename/path handling | P0 | AB Download Manager + validation | No traversal, unexpected overwrite or reserved-name failure |
| D08 | Integrity/checksum validation | P0 | AB Download Manager / own adapter | Invalid hash reported; file not marked successful |
| D09 | Segmented transfer when supported | P1 | AB Download Manager | No duplicated bytes; compare with single connection fairly |
| D10 | Scheduling and speed limits | P1 | AB Download Manager | Jobs start at intended local time; limits measured |
| D11 | Download categories, search and history | P1 | AB Download Manager | History survives restart and can be searched |
| D12 | Proxy and network settings | P1 | AB Download Manager | Explicit configured proxy works, credentials remain protected |
| D13 | Bulk link intake, deduplication | P1 | ReClip / AB | Repeated URLs detected; deliberate duplicates still possible |
| D14 | Import/export of job lists | P2 | Design needed | Versioned format with invalid data handled safely |
| D15 | CLI, local REST API | P2 | Gopeed/Motrix reference | Authenticated, versioned and documented API |

## B. Browser companion

| ID | Feature | Priority | Candidate | Acceptance target |
| --- | --- | --- | --- | --- |
| B01 | Desktop handoff for direct downloads | P0 | AB Browser Integration | User click creates exactly one authenticated desktop job |
| B02 | Popup and right-click controls | P0 | AB Browser Integration | Works on supported Chrome/Edge version |
| B03 | Safe native messaging/pairing | P0 | AB integration + security work | Arbitrary page cannot command local helper |
| B04 | Active job progress | P1 | AB integration / adapter | Popup reflects state from desktop engine |
| B05 | Supported direct media detection | P1 | MediaGrabber | Correct media listed with user-initiated action |
| B06 | Authorized non-DRM HLS/DASH | P1 | HLS Downloader | Tested with owned sample stream and correct audio/video |
| B07 | Firefox extension support | P1 | AB integration | Package and smoke-test in supported Firefox |
| B08 | Fine-grained host permissions | P0 | Manifest V3 | Minimum requested permissions justified and reviewed |
| B09 | Optional remote target | P2 | Gopeed reference | Secure pairing, explicit consent and no open control port |

## C. Media conversion and organization

| ID | Feature | Priority | Candidate | Acceptance target |
| --- | --- | --- | --- | --- |
| M01 | Import user-owned local video/audio | P1 | New media module | File processed offline without uploading by default |
| M02 | MP4/MKV/WebM remux where compatible | P1 | FFmpeg | Container conversion plays with synchronized audio |
| M03 | MP3/M4A/FLAC/WAV/Opus extraction | P1 | FFmpeg | Metadata/sample duration checked; lossless output not mislabeled |
| M04 | Video compression and presets | P1 | FFmpeg | Actual output codec/quality/size displayed |
| M05 | Trim/merge permitted media | P1 | FFmpeg | Audio stays synchronized; invalid edits fail safely |
| M06 | Subtitle import/export | P1 | FFmpeg / media metadata | Supported SRT/VTT conversions tested |
| M07 | Audio-track and language selection | P1 | FFmpeg | Selected track matches requested language when metadata exists |
| M08 | Batch conversion queue | P1 | Shared jobs | Job failures do not corrupt remaining jobs |
| M09 | Available 4K/8K processing | P1 | FFmpeg | No false promise of native 8K when source is lower quality |
| M10 | Metadata and media library | P2 | Parabolic/Tube Archivist reference | Search, thumbnails, duplicate handling, privacy |
| M11 | AI transcription and tagging | P2 | Unselected | Opt-in, privacy and cost disclosures; measurable value |
| M12 | Site-specific connector architecture | P1 | yt-dlp optional | Explicit allowlist/rights review and connector regression tests |

## D. Website, platform and release engineering

| ID | Feature | Priority | Candidate | Acceptance target |
| --- | --- | --- | --- | --- |
| W01 | Public product/installation website | P1 | New site / existing marketing pattern | Accessible and mobile responsive |
| W02 | Uploaded media conversion | P1 | Original hardened service/FFmpeg; ReClip UX reference | Size/format limits, authenticated cleanup and valid output |
| W03 | Permitted direct URL job UI | P1 | Original hardened design; ReClip UX reference | SSRF-safe URL checks, explicit quotas, progress |
| W04 | Cloud/server-side queue | P2 | Redis/worker candidate | Bounded resource use, cancellation and TTL cleanup |
| W05 | User accounts and billing | P2 | Not selected | Only after privacy policy and business model approval |
| P01 | Reproducible developer builds | P0 | AB build + extension tooling | Fresh environment builds without private dependencies |
| P02 | Automated unit/integration tests | P0 | Project-specific | Core state machine, security cases, extension bridge covered |
| P03 | Signed release and installer | P1 | Existing packager | Install/update/uninstall tested on supported OS |
| P04 | Auto-updates and rollback strategy | P1 | Existing updater | Signature verification and controlled release channels |
| P05 | Dependency/license SBOM | P0 | Standard tooling | All shipped dependencies and licenses documented |
| P06 | Privacy-safe logs and diagnostics | P0 | New/shared | No credentials, cookies or private URLs in logs |
| P07 | Localization and accessibility | P1 | App i18n tooling | Keyboard navigation, labels, contrast, locale correctness |
| P08 | Plugin SDK and marketplace | P2 | Motrix/Gopeed reference | Signed plugins, sandbox, granular permissions |

## Release gates

- **P0 beta:** One supported desktop OS and Chrome, authenticated handoff, reliable authorized HTTPS file download, integrity/recovery tests, license audit and build/release docs.
- **P1 commercial-quality candidate:** media processing, supported HLS/DASH, quality/format selection, better UX, localization, signed updates and supported website workflows.
- **P2 expansion:** automation APIs, mobile/remote access, optional AI, advanced plugins and cloud services.

## Tracking rule

When code arrives, add columns or an issue-linked manifest for **local implementation status**, **last verified commit**, **test command**, **platform** and **evidence**. Never change an item to "Done" solely because the upstream README mentions it.

<!-- END docs/FEATURE_MATRIX.md -->


---

# SOURCE 8: docs/ARCHITECTURE.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/ARCHITECTURE.md  
Blob: `ae60f91ca7150324ee3847dacd1b6e5a15413915`

<!-- BEGIN docs/ARCHITECTURE.md -->

# Proposed architecture (design, not implementation)

> **Draft.** The initial implementation has not been chosen or imported. Confirm feasibility with source inspection before making framework-specific commitments.

## Core approach

**Desktop first, local by default, one final project repository.** The desktop engine owns download jobs and writes files locally. The browser extension only discovers authorized links and sends authenticated, explicit user-initiated jobs. Media processing is a separate component using FFmpeg for supported files. The website begins as a product presence and expands to a secure optional web service later.

~~~mermaid
flowchart TD
  Extension["Browser companion"] -->|"paired request"| Bridge["Local authenticated API"]
  Desktop["Desktop UI"] --> Engine["Download/job engine"]
  Bridge --> Engine
  Engine --> Files["Local storage + history"]
  Engine --> Media["Media processor"]
  Web["Optional web app"] --> Hosted["Isolated hosted API/queue"]
  Hosted --> Media
~~~

The local engine should not depend on the hosted service to download direct files. The hosted API must not accept arbitrary unrestricted URLs.

## Preferred first baseline

- **Desktop:** Investigate AB Download Manager (Kotlin Multiplatform/Compose) **as-is** before choosing a rewrite.
- **Extension:** Investigate AB Browser Integration and its native/localhost handoff.
- **Web UI:** Evaluate ReClip separately. It is **not** already integrated into AB.
- **Media processor:** Wrap trusted FFmpeg invocation with argument arrays, timeouts, resource limits, job cancellation and sanitized metadata.
- **Source plugins:** Permissioned adapters for explicitly supported sources; never assume raw media downloads are permitted merely because yt-dlp offers an extractor.
- **Persistence:** First reuse the chosen desktop application's storage. Add PostgreSQL/Redis only if a later hosted service genuinely needs them.

**Avoid premature stack conflict:** Earlier research considered Next.js/FastAPI/Tauri for a greenfield product. The newer AB foundation is Kotlin, and preserving that tested code is the current preferred experiment. A framework rewrite requires evidence that integration is more costly than migration.

## Domain boundaries

| Module | Owns | Does not own |
| --- | --- | --- |
| Download engine | Transfers, resume/retry, concurrency, state, integrity | Scraping sites, UI rendering |
| Browser extension | User intent, permitted URL discovery, secure handoff | Secret storage, FFmpeg execution, arbitrary filesystem paths |
| Local bridge | Pairing, auth, rate limits, state/read-only APIs | Open unauthenticated public access |
| Media processor | Local file conversions, media probing, safe output handling | Source rights decisions or browser cookie extraction |
| Source connector | Rights-reviewed metadata/source resolution | Privilege escalation, DRM circumvention |
| Web service (later) | Quotas, auth, bounded jobs, cleanup, abuse protection | Unrestricted server-side URL fetching |

## Job contract (proposed)

A job should carry at least:

| Field | Meaning |
| --- | --- |
| id | Stable UUID |
| source_kind | direct_url, local_file, or approved_connector |
| source | Safe reference, not a secret-bearing URL in persistent logs |
| destination | Validated, user-selected destination |
| format_profile | Explicit conversion/quality options or original |
| created_at / updated_at | Time tracking |
| status | Lifecycle state |
| bytes_done / bytes_total | Progress, total may be unknown |
| checksum | Optional integrity expectation |
| retry_count / error_code | Recovery and user messaging |
| provenance | Originating UI, permission decision and source adapter version |

Proposed lifecycle:

~~~mermaid
stateDiagram-v2
  [*] --> Queued
  Queued --> Resolving
  Resolving --> Downloading
  Downloading --> Verifying
  Verifying --> Processing
  Verifying --> Complete
  Processing --> Complete
  Downloading --> Paused
  Paused --> Queued
  Resolving --> Failed
  Downloading --> Failed
  Verifying --> Failed
  Processing --> Failed
  Failed --> Queued: retry
  Queued --> Cancelled
  Downloading --> Cancelled
  Complete --> [*]
  Cancelled --> [*]
~~~

Workers must be idempotent where feasible; interrupted jobs must persist recoverable state. Successful completion means output is committed safely and integrity requirements passed, not just that a network transfer ended.

## Extension-to-desktop security contract

1. The user explicitly selects a URL or initiates a permitted capture.
2. Companion checks allowed origins and authenticates to a paired local helper; no wildcard CORS.
3. Helper validates URL scheme and destination, enforces limits and creates one job.
4. Extension receives a job ID and polls/subscribes to permitted progress fields.
5. Credentials and sensitive request headers remain local and are never echoed to logs or remote websites.

Prefer native messaging or a well-secured loopback bridge after auditing AB's existing protocol; do **not** design a privileged localhost API open to arbitrary tabs.

## Future monorepo layout (illustrative)

~~~
apps/
  desktop/             # selected existing desktop foundation
  extension/           # companion browser integration
  web/                 # optional website
components/
  media-processor/     # local FFmpeg wrapper
  source-connectors/   # rights-reviewed adapters
contracts/             # API schema, versioning, generated clients
tests/
  fixtures/            # small redistributable test files
  integration/
docs/
infrastructure/        # only when needed for hosting
~~~

The folder structure can change after the audit; keep integration **interfaces stable and documented**, not rigidly fixed directory names.

<!-- END docs/ARCHITECTURE.md -->
