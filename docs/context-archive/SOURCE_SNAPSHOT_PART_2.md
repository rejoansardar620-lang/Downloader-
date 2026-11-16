# SOURCE DOCUMENT SNAPSHOT — Part 2 of 3

Dated: 2026-11-16. Original documents reproduced from GitHub. These are immutable historical snapshots, not live current-state claims.

---

# SOURCE 9: docs/ROADMAP.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/ROADMAP.md  
Blob: `3ebabf3cfbdaeab72ac9831ca95813fca36e1fb0`

<!-- BEGIN docs/ROADMAP.md -->

# Roadmap with release gates

The milestones below are **planned**, not completed. Every milestone ends with a reviewable commit/PR, commands run, test evidence and updated feature status. Do not merge all upstream projects at once.

## M0 — Audit, choices and legal foundation (first task)

**Outputs:** build/license/dependency/security reports for AB desktop + browser integration and alternative candidates; feature coverage matrix; verified sample test assets; proposed architecture; explicit owner decisions on license and branding.

**Exit gate:** One primary base chosen with evidence, license obligations documented, initial OS/browser chosen, risk register reviewed. **No implementation rewrite yet.**

## M1 — First working desktop vertical slice (P0)

**Scope:** Build selected desktop foundation; download an authorized HTTPS file, show progress, pause/resume where supported, retry, checksum verification, restart recovery, safe filename handling.

**Exit gate:** Reproducible clean install/build; automated local integration tests; no corruption on interrupted transfer; actual output hash matches fixture.

## M2 — Browser handoff and packaging (P0)

**Scope:** Build selected browser companion; context menu and download capture; pair with desktop; authenticated job handoff; status feedback; browser permissions review.

**Exit gate:** Chrome-to-desktop demo succeeds; malicious webpage cannot trigger unauthorized local action; extension package generated; permission inventory documented.

## M3 — Owned-media processing (P1)

**Scope:** FFmpeg probing, audio extraction, MP4/MKV/WebM, non-DRM HLS/DASH from permitted test sources, subtitles, batch media processing, cancellation and resource limits.

**Exit gate:** Deterministic sample fixtures pass; audio/video sync and metadata verified; media failures do not hang UI; shipped FFmpeg binary license recorded.

## M4 — Website experience (P1)

**Scope:** Product site, download/install guides, optional upload-based converter or rights-reviewed direct URL workflow, clear privacy/terms, accessibility.

**Exit gate:** Security review (SSRF, quotas, uploads, retention), responsive usability tests and documented hosting cost; public deployment only by owner approval.

## M5 — Production-ready release (P1)

**Scope:** CI for supported OS/browser, signed installers and updates, telemetry opt-in, privacy-safe crash reports, localization, release notes, rollback plan, SBOM, security disclosures.

**Exit gate:** Clean-machine installation and update tests, accepted security findings, release checklist and support docs.

## M6 — Competitive expansion (P2)

**Possible scope:** Plugin SDK, CLI/API, scheduled subscriptions for permitted sources, advanced file sorting, remote jobs, mobile companion, AI-assisted indexing of owned media.

**Exit gate:** Product usage and support evidence justify each major feature. No automatic commitment to cloud cost.

## How to prevent credit waste

For every milestone, maintain this compact progress table in the issue/PR:

| Feature | Upstream status | Local status | Evidence | Next work |
| --- | --- | --- | --- | --- |
| Example: queue | Found in AB docs | Not implemented here | No local test | Audit source/build |

Rules:

1. Complete M0 before choosing a large rewrite.
2. Work on one milestone and a few acceptance criteria per task.
3. Run relevant existing tests before adding new code.
4. Reuse verified packages, keep dependency caches and avoid live-network media tests in every build.
5. Stop for a decision if a license, platform-policy or material architecture blocker appears.
6. Do not report or advertise a release until its exit gate is satisfied.

<!-- END docs/ROADMAP.md -->


---

# SOURCE 10: docs/DECISIONS.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/DECISIONS.md  
Blob: `7356fa05e1185556a538237bf90be6830682f60a`

<!-- BEGIN docs/DECISIONS.md -->

# Architectural decision register

This records preferences, **not immutable architecture**. Each decision must be updated after the audit and approved by the project owner where indicated.

| ID | Decision | Status | Rationale / next action |
| --- | --- | --- | --- |
| ADR-001 | Desktop-first, local-first product with web + browser companions | Proposed | Strong baseline for download reliability and reduced server cost |
| ADR-002 | AB Download Manager as primary desktop candidate | **Recommended after source triage; owner approval pending** | At pinned 68f0ea6, existing Kotlin queue/part DB and native host; Gradle/JDK 25 build and full dependency review unresolved |
| ADR-003 | AB Browser Integration as first extension candidate | **Recommended after source triage; owner approval pending** | At pinned bf6811d, paired with AB; Chrome/Firefox bundles PASS but TypeScript checks FAIL and bridge requires security hardening |
| ADR-004 | ReClip as website candidate | **Reference only; adoption deferred** | MIT Flask UI patterns useful; arbitrary URL processing without sufficient evident SSRF, quota, authentication and persistence safeguards blocks hosted adoption |
| ADR-005 | MediaGrabber/HLS Downloader as selective additions | Proposed | Review only components needed for permitted media formats |
| ADR-006 | FFmpeg for media conversion | Proposed | Established tool, but distribution/configuration licensing varies |
| ADR-007 | yt-dlp as optional restricted connector | Proposed | No guarantee of site availability or platform permission |
| ADR-008 | No blanket GPL/AGPL code import into proprietary app | Proposed | Distribution and network-source obligations require owner/legal decision |
| ADR-009 | Project license / contributor terms | **Owner decision required** | Public GitHub visibility is not a software license; do not invent one |
| ADR-010 | Brand name, logos, domains and monetization | **Owner decision required** | Distinct identity and trademark review needed |
| ADR-011 | Initial OS/browser: Windows and Chrome | Recommended, unapproved | Efficient first end-to-end test pair |
| ADR-012 | Greenfield Tauri/Next.js rewrite | **Deferred** | Reassess after building the existing Kotlin-based candidate |
| ADR-013 | Cloud storage, Redis/PostgreSQL and billing | **Deferred** | Introduce only with a proven hosted use case |
| ADR-014 | Public media-download sources and allowed URL policy | **Owner/legal decision required** | Platform policies and distribution constraints differ by source |

## Decision template

~~~
ID:
Date:
Owner:
Context:
Options:
Decision:
License/security implications:
Evidence/tests:
Revisit when:
~~~

Changes to these decisions should be made via an issue or PR with explicit reasoning and review. Avoid committing to a framework because it appears in an example diagram.

**Evidence (15 November 2026):** The [17-project source/reuse matrix](REUSE_DECISION_MATRIX.md), [M0 audit](M0_AUDIT.md) and [build-start plan](BUILD_START_PLAN.md) document the rationale. These are **recommendations, not owner approvals**. Apache-2.0 root license declarations do not replace transitive dependency, NOTICE, asset, distribution and trademark reviews. Do not copy the upstream application code until the governance gate is recorded.

<!-- END docs/DECISIONS.md -->


---

# SOURCE 11: docs/LEGAL_SECURITY.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/LEGAL_SECURITY.md  
Blob: `cb87e6469f6df35b5badcc1ae6b14039cb31ad63`

<!-- BEGIN docs/LEGAL_SECURITY.md -->

# Legal, licensing, privacy and security guardrails

This is engineering guidance, **not legal advice**. Get appropriate professional review before releasing a paid product, distributing third-party binaries, or supporting a new platform.

## Platform permission boundaries

1. **YouTube terms:** Do not assume that being able to view a video grants a right to download it. Terms restrict reproducing/downloading content except where specifically allowed by the service, applicable law, or relevant permissions.
2. **YouTube API policies:** The YouTube API is not a general audiovisual download API. Its rules restrict downloading, caching and offline use without prior written approval.
3. **Chrome Web Store:** The store restricts extensions facilitating YouTube downloads; a self-installed development extension is **not** evidence that an extension is eligible for store publication.
4. **Copyright and authorization:** Support direct downloads/uploads and other sources only where the user has rights and the service permits the operation. Do not provide DRM, paywall, authentication, anti-bot or region-control bypass features.
5. **Consent and account access:** Never extract user cookies, browser credentials or private tokens without informed consent and an approved design; prefer platform-supported authentication and minimize data handling.

Official sources:

- https://www.youtube.com/t/terms
- https://developers.google.com/youtube/terms/developer-policies
- https://developer.chrome.com/docs/webstore/troubleshooting
- https://developer.chrome.com/docs/webstore/program-policies/malicious-and-prohibited
- https://support.google.com/youtube/answer/56100 (official download route for one's own uploads)

## License governance

Before using third-party code or shipping binaries:

- Record upstream source, commit, exact license, copyright holders, NOTICE and bundled dependencies.
- Generate an SBOM and preserve required license/attribution notices in source and releases.
- Inspect FFmpeg build flags/codecs carefully (LGPL versus GPL/nonfree configurations).
- Treat AGPL/GPL and other copyleft code as **reference only** unless the owner approves obligations and code-use design.
- Do not copy logos, product names, graphics, protected UI designs or documentation wholesale.
- Do not assume GPT/Codex-generated output is uniquely owned or free of third-party license issues.
- **The project owner has not chosen a license for original Downloader code.** Public visibility alone is not permission to reuse it commercially.

## Threat model: desktop and browser

| Threat | Requirement |
| --- | --- |
| Malicious website commands local helper | Pairing token/native messaging, strict origin/extension IDs, no wildcard CORS, user confirmation |
| Arbitrary filesystem access | Validate destination, canonicalize paths, limit writable directories |
| Unsafe source URL or redirect | Validate each hop, deny local/loopback/link-local/private/reserved endpoints and DNS rebinding |
| Untrusted downloaded filenames | Sanitize path separators, control characters, reserved names and collision behavior |
| Credential leakage | Secret store/OS keychain; redact logs; short-lived tokens; no cookie dumping |
| Malicious media file or playlist | Resource-limited workers, trusted FFmpeg, updates, timeouts and crash isolation |
| Unauthorized software updates | Signed builds, verified update metadata, rollback and safe release channels |
| Extension over-permission | Manifest V3, minimized host permissions, clear consent, automated permission review |
| Data loss | Atomic output rename, checksum, safe temporary-file handling, explicit cleanup |
| Supply-chain compromise | Pin/review dependencies, SBOM, vulnerability scanning, provenance and CI protection |

## Threat model: optional hosted services

- Block SSRF to cloud metadata, RFC1918, loopback, link-local, IPv6 internal and localhost hostnames.
- Validate redirects and DNS answers on every request; do not proxy privileged credentials.
- Restrict protocols, response size, download duration, concurrency, accepted content types and disk quota.
- Authenticate users; set per-user and global rate/cost limits; provide job expiry and media deletion.
- Never expose storage buckets or private media publicly by default.
- Make data retention, privacy policy, abuse handling and takedown contact available before launch.
- Prefer local processing to avoid paying for other users' bandwidth and to reduce privacy risks.

## Minimum verification before every release

- Rights/permissions and allowed-source checklist.
- Dependency and license scan; binary provenance.
- Secret scanning and browser permission diff.
- Tests for corrupted downloads, redirects, SSRF, malicious paths, retry storms and disk exhaustion.
- Clean-machine install/update/uninstall and browser-bridge abuse tests.
- Privacy text, security contact and third-party notices updated.

Do not deploy, publish a browser-store listing or claim copyright compliance until these gates are reviewed.

<!-- END docs/LEGAL_SECURITY.md -->


---

# SOURCE 12: docs/DEVELOPMENT_SETUP.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/DEVELOPMENT_SETUP.md  
Blob: `22d7bc77732a8ccbac9a5c7c31c13b2f528c5ee9`

<!-- BEGIN docs/DEVELOPMENT_SETUP.md -->

# Development and verification guide

**As of the 15 November 2026 M0 audit, there is no user-facing application to run here.** The original HTTP/HTTPS fixture and automated tests are runnable. The upstream candidates are not yet imported.

## Minimum prerequisites for initial audit

- Git and GitHub access.
- A clean disposable development environment for building unfamiliar upstream code.
- JDK/Gradle tooling appropriate for [AB Download Manager](https://github.com/amir1376/ab-download-manager).
- Node.js/npm for [AB Browser Integration](https://github.com/amir1376/ab-download-manager-browser-integration).
- FFmpeg only for **permitted test media** during the later media milestone.
- Windows/Chrome test access for the first end-to-end bridge; use Windows CI runners if developing from Linux cloud.

Avoid installing arbitrary scripts as administrator or executing unchecked third-party CI secrets. Read build files first, pin dependency versions and use standard package checks.

## Getting the project docs

~~~
git clone https://github.com/rejoansardar620-lang/Downloader-.git
cd Downloader-
~~~

Read in order: README.md, AGENTS.md, docs/START_HERE.md, docs/FEATURE_MATRIX.md, docs/LEGAL_SECURITY.md, docs/ROADMAP.md.

## Upstream audit worktrees (do not import yet)

Research candidates in separate, isolated directories. The repository links and initial priorities are listed in [REFERENCE_REPOSITORIES.md](REFERENCE_REPOSITORIES.md).

For AB Download Manager, the upstream README documents a Gradle release task:

~~~
./gradlew createReleaseFolderForCi
~~~

The upstream browser integration documents:

~~~
npm install
npm run pack:chrome
npm run pack:firefox
~~~

**These are upstream instructions, not commands already run in this repository.** Verify JDK requirements, dependencies, scripts, network policy and expected outputs first.

## First integration test (proposed)

The first synthetic [loopback HTTP fixture](../tools/fixtures/README.md) is implemented. From the repository root, run:

~~~sh
python3 -m unittest discover -s tests -v
python3 -m tools.fixtures.http_fixture --port 8765
~~~

Use `http://127.0.0.1:8765/file.bin` for Range-aware tests and `/no-range.bin` to test safe full restart. The fixture has **14 passing tests** covering Range/If-Range, redirects, truncated transfers and certificate-validated HTTPS. It supports optional caller-supplied TLS cert/key (see its own README); no desktop downloader has consumed it. The following desktop-to-browser end-to-end test remains proposed.

1. Serve a small openly redistributable test file from a controlled HTTPS endpoint that supports Range requests.
2. Download to a user-selected temporary directory.
3. Compare SHA-256 hash with the fixture.
4. Interrupt the connection and restart/resume.
5. Send the same job through the Chrome extension and local bridge.
6. Attempt a malicious/unauthorized cross-origin bridge request and verify rejection.
7. Record commands, OS/browser versions, pass/fail outcomes and screenshots where useful.

Use synthetic or clearly licensed media; do **not** use arbitrary YouTube videos as default automated tests.

## When code is introduced

Every module should document:

- Installation and commands for Linux/Windows/macOS where supported.
- Local build/test/lint/typecheck commands and expected output.
- Environment variable examples without secrets.
- Test fixture licensing and storage limits.
- Runtime/debugging/telemetry behavior.
- Extension IDs, native messaging registration, endpoint binding and pairing.
- CI job links and release artifact provenance.

## CI recommendations

| Stage | Initial check |
| --- | --- |
| PR (fixture only, implemented) | Python unittest/compileall; pinned GitHub actions, no secrets or external media |
| PR (future app) | Formatting, static analysis, unit tests, secrets, dependency/license review |
| Integration (future) | Local HTTP/HTTPS fixture, job lifecycle, cross-origin bridge security |
| Browser (future) | Build Chrome MV3 and smoke-test supported browser |
| Desktop (future) | Linux build + Windows CI build; separate signed release job |
| Release (future) | SBOM, third-party notices, artifacts, hashes, signature/rollback checks |

Use dependency caching and run narrow checks during development. A full multi-OS release build should run at milestone gates, not after every documentation change.

## Current verification status

- GitHub repository: planning/docs and original fixture plus initial test workflow; **no application implemented**.
- Desktop build: **blocked** at Gradle 9.8 bootstrap by release-asset network restriction; upstream source specifies JDK 25 toolchain. See [issue #4](https://github.com/rejoansardar620-lang/Downloader-/issues/4).
- Extension (isolated upstream clone): **Chrome and Firefox bundle builds PASS**, standalone TypeScript **FAILS**. See [issue #5](https://github.com/rejoansardar620-lang/Downloader-/issues/5).
- Browser integration: **not attempted end-to-end**; upstream unauthenticated-by-default HTTP API is a release blocker. See [issue #6](https://github.com/rejoansardar620-lang/Downloader-/issues/6).
- Cloud deployment: **not attempted**.
- Tests: **14/14 local Python fixture tests PASS**; upstream desktop tests not run; new GitHub Actions workflow not yet verified on GitHub.

See [BUILD_START_PLAN.md](BUILD_START_PLAN.md) and [REUSE_DECISION_MATRIX.md](REUSE_DECISION_MATRIX.md) before importing or rewriting source.

<!-- END docs/DEVELOPMENT_SETUP.md -->


---

# SOURCE 13: docs/CODEX_WORKFLOW.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/CODEX_WORKFLOW.md  
Blob: `9c7503fe8b5b829ad4467ca1662601438f01deb9`

<!-- BEGIN docs/CODEX_WORKFLOW.md -->

# Codex Cloud workflow: build efficiently

The owner intends to use GPT-6 Astra/Codex Cloud where available. Model access, task allowances, network access and GitHub permissions depend on the selected environment.

## Start in the correct repository

1. Connect GitHub to Codex and grant access to **rejoansardar620-lang/Downloader-**.
2. Create/select a Codex Cloud environment containing this repository.
3. Configure required development tools, dependency caches and only the network domains genuinely needed.
4. Give Codex a **single bounded milestone**, not "build every feature".
5. Inspect its diff, tests, risks and billable cloud dependencies before approving further work.
6. Commit/push important work. A new cloud task may not inherit uncommitted changes from an earlier task.

Official overview: https://help.openai.com/en/articles/20001545-using-codex-cloud

## First prompt: audit only

~~~text
You are the lead engineer for Downloader.
Read README.md, AGENTS.md, docs/START_HERE.md,
docs/PRODUCT_VISION.md, docs/FEATURE_MATRIX.md,
docs/REFERENCE_REPOSITORIES.md, docs/ARCHITECTURE.md,
docs/ROADMAP.md, docs/LEGAL_SECURITY.md and docs/DECISIONS.md.

GOAL: Build a reliable commercial-quality desktop-first download
manager, browser companion and optional authorized media web UI.
Minimize coding time, compute costs and duplicated functionality.

THIS TASK IS M0 ONLY. DO NOT BUILD THE ENTIRE PRODUCT.

Audit AB Download Manager and AB Browser Integration first.
Compare ReClip, MediaGrabber, HLS Downloader, Parabolic and the
other repositories in docs/REFERENCE_REPOSITORIES.md as needed.

For each candidate:
- Pin the exact commit and read license/third-party notices.
- Check actual source and existing tests, not README alone.
- Build in a safe isolated environment if feasible.
- Record framework, supported platforms, license obligations,
  dependencies, maintenance and security risks.
- Map requested features to verified upstream / partial / missing.
- Keep local repository status distinct from upstream capabilities.
- Propose a single smallest Windows + Chrome HTTPS direct-file
  download vertical slice with measurable acceptance tests.

Update the audit report and docs/DECISIONS.md with proposed choices.
Report every command run and whether it passed or was blocked.
Do not copy upstream code, deploy, buy services or expose secrets.
Stop after M0 and request owner approval for M1.
~~~

## Follow-up prompt for an approved implementation milestone

~~~text
Implement only milestone [M1/M2/etc] from docs/ROADMAP.md,
using the owner-approved ADRs in docs/DECISIONS.md.

First inspect existing source/tests and list what is reusable.
Preserve licenses/notices and maintain clear module boundaries.
Do not build features already functioning in the chosen base.
Add deterministic tests with authorized fixtures, then run the
relevant build and security checks.

Update docs/FEATURE_MATRIX.md with only verified results.
Provide diff summary, test commands/results, unresolved blockers,
security and licensing changes, and a short next-step recommendation.
Stop at the milestone boundary.
~~~

## Anti-waste rules

| Bad pattern | Better instruction |
| --- | --- |
| "Build the whole competitor now" | One milestone with 3–6 measurable acceptance criteria |
| Reimplement queues already in AB | Audit and adapt existing queue APIs |
| Import 10 repositories in one commit | Pin and assess, then import minimal reviewed components |
| Run full video compatibility on every change | Offline fixture tests; bounded scheduled compatibility tests |
| Switch from Kotlin to Tauri without evidence | Build the existing desktop base first, then compare costs |
| Claim everything works because it compiles | Validate behavior end-to-end on supported OS/browser |
| Rebuild all platforms on every tiny edit | Targeted tests first, full CI at release gates |
| Pay for servers before product validation | Local-first architecture, hosting only after approval |

## Reporting format expected from Codex

1. **Outcome:** what is now demonstrably usable?
2. **Changed files:** bounded list.
3. **Feature status:** verified complete / partial / blocked / not started.
4. **Checks:** commands, environment, outcomes and limitations.
5. **Licenses/security:** new dependencies or risks.
6. **Next milestone:** one proposed bounded task.

## Boundaries

- Do not bypass DRM, paywalls, account protections, YouTube/Chrome policies or unauthorized source restrictions.
- Do not harvest browser cookies or repackage third-party source without authorization/license review.
- Do not use hidden credentials, create paid resources, or publish releases without the owner's approval.
- Do not obscure failures: a network-blocked test is **blocked**, not passed.

<!-- END docs/CODEX_WORKFLOW.md -->


---

# SOURCE 14: docs/IMPLEMENTATION_CHECKLIST.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/IMPLEMENTATION_CHECKLIST.md  
Blob: `3819c8793fc9c6f6b8e06723793789a1772bdc71`

<!-- BEGIN docs/IMPLEMENTATION_CHECKLIST.md -->

# Complete ordered implementation checklist

**Project:** Downloader — independently branded desktop-first download manager, browser companion and authorized media-processing product.

**Last checked:** 15 November 2026. See the [updated engineering audit](M0_AUDIT.md), [17-source matrix](REUSE_DECISION_MATRIX.md) and [build-start plan](BUILD_START_PLAN.md) for exact pins, build evidence, security findings and continuation instructions.

**Status rule:** Checked items have evidence in this repository or a linked issue/PR. All other items are **not verified**. A feature present in an upstream repository is not yet implemented here.

**Execution order:** Complete each gate before starting the next milestone. The project owner may re-prioritize tasks, but record the decision first. Refer to [feature IDs](FEATURE_MATRIX.md), [roadmap](ROADMAP.md), [AGENTS.md](../AGENTS.md), and [security requirements](LEGAL_SECURITY.md).

## 0. Governance and project setup — BLOCKING DECISIONS

- [x] G00: Create GitHub repository with initial documentation and contributor instructions — [bootstrap commit](https://github.com/rejoansardar620-lang/Downloader-/commit/51022e8f6440c3dabba46179c5642a7c8c3d3a3e).
- [x] G01: Write vision, competitor benchmarks, architecture proposal, roadmap, and feature matrix.
- [x] G02: Add AGENTS.md, CONTRIBUTING.md, SECURITY.md, issue/PR templates and initial M0 issues.
- [ ] G03: Owner chooses **product name**, logo policy, trademark review and public project description.
- [ ] G04: Owner decides **software license and contribution terms** before accepting significant third-party or contributor code. Do not silently choose one.
- [ ] G05: Owner approves initial target **Windows 11 + Chrome** (recommended); set secondary OS/browser priorities.
- [ ] G06: Owner confirms initial product scope: desktop-first authorized direct downloads, then media tools, then web service.
- [ ] G07: Define versioning, issue labels, branch protection and PR review ownership.
- [ ] G08: Set an explicit cost ceiling for Codex/CI/cloud testing and deployment.

**Gate G:** Decisions G03–G06 documented in [DECISIONS.md](DECISIONS.md) by the owner. Auditing can proceed before this gate; code imports and public releases cannot.

## 1. M0 — Research, audits and reuse decision (first engineering milestone)

### 1A. Desktop and extension foundations

- [x] A01: Pin exact upstream commit SHA/tag for [AB Download Manager](https://github.com/amir1376/ab-download-manager) — [audit](M0_AUDIT.md) pins `68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68`.
- [ ] A02: Inspect desktop source modules, build scripts, supported OS matrix and configuration.
- [ ] A03: Audit Apache-2.0 obligations and all transitive dependencies, including bundled native tools.
- [ ] A04: Determine existing download features by **source + tests**: D01–D12.
- [ ] A05: Build AB desktop in an isolated environment; record JDK, Gradle command, OS and actual result.
- [ ] A06: Run upstream tests; classify pass/fail/blocked and investigate critical failures.
- [x] A07: Pin exact upstream commit SHA/tag for [AB Browser Integration](https://github.com/amir1376/ab-download-manager-browser-integration) — [audit](M0_AUDIT.md) pins `bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05`.
- [ ] A08: Audit extension permissions, Manifest V3 compatibility, browser store restrictions and native messaging/localhost security.
- [ ] A09: Build extension packages and run available tests; record evidence — Chrome MV3 and Firefox bundler builds PASS; standalone `tsc --noEmit` FAIL; browser installation and package verification remain. See [audit](M0_AUDIT.md), [blocker #5](https://github.com/rejoansardar620-lang/Downloader-/issues/5).
- [ ] A10: Validate the existing desktop-to-extension protocol, or document a safe adaptation strategy.

### 1B. Media, website and alternatives

- [ ] A11: Pin and audit [ReClip](https://github.com/averygan/reclip) (website). Verify actual batch, format and quality behavior.
- [ ] A12: Pin and audit [MediaGrabber](https://github.com/miroshArtem/MediaGrabber) and [HLS Downloader](https://github.com/puemos/hls-downloader) (extension/media security).
- [ ] A13: Assess [Parabolic](https://github.com/NickvisionApps/Parabolic) versus AB: supported platforms, build quality, and reuse costs.
- [ ] A14: Audit FFmpeg executable build/license and allowable code paths for supported media-processing features.
- [ ] A15: Audit yt-dlp and bundled license requirements for **optional** use on authorized sources only.
- [ ] A16: Study Gopeed, Motrix/aria2, Cobalt, MeTube and Tube Archivist for **feature ideas only**, unless their source license is explicitly approved.
- [ ] A17: Create a dependency/license/SBOM risk register with exact source commits and NOTICE obligations.
- [ ] A18: Benchmark baseline binaries with controlled legal fixtures; record CPU/RAM, resume reliability and integrity rather than marketing speed claims — [HTTP/HTTPS fixture](../tools/fixtures/README.md) has 14 passing local tests, including interruption/redirect/If-Range; **no application benchmark**.
- [ ] A19: Produce a **build vs reuse matrix** for every P0/P1 feature, including cost/risk of integration.
- [ ] A20: Compare one-core/one-extension architecture to greenfield rewrite. Recommend a single base with evidence.
- [ ] A21: Update DECISIONS.md and FEATURE_MATRIX.md with **verified**, not presumed, results.

**Gate M0:** Owner approves a base/extension, software license path, exact pinned upstream commits, first fixture and M1 acceptance criteria. No broad repository merger.

## 2. M1 — Local download engine / first runnable product (P0)

- [ ] D01: Import or integrate only approved, pinned upstream source and preserve attribution/NOTICE.
- [ ] D02: Create the maintainable repository/module structure without breaking upstream history or tests.
- [ ] D03: Establish hermetic/reproducible developer environment and build scripts.
- [ ] D04: Add small, explicitly redistributable HTTPS fixtures (including Range-capable and Range-disabled servers) — original [HTTP/optional HTTPS loopback fixture](../tools/fixtures/README.md) and unit tests implemented; full application integration and controlled remote HTTPS validation pending.
- [ ] D05: Implement/verify direct HTTPS download to user-approved local directory.
- [ ] D06: Show real bytes, progress, unknown-length support, speed and errors.
- [ ] D07: Implement/verify pause/resume where HTTP Range is supported; safe restart otherwise.
- [ ] D08: Implement/verify retry, backoff, cancellation and partial-file cleanup.
- [ ] D09: Implement/verify bounded concurrent jobs, queue order and prioritization.
- [ ] D10: Implement/verify safe filenames, collision policy, disk-full behavior and path validation.
- [ ] D11: Implement/verify file integrity check and atomic finalization.
- [ ] D12: Persist job history and recover from network failure, app crash and restart.
- [ ] D13: Add unit tests for state transitions, redirects, retries, corruption and cancellation.
- [ ] D14: Run real desktop application with a test fixture; capture reproducible test evidence.
- [ ] D15: Document build, run, debug, uninstall and known limitations.

**Gate M1:** Clean Windows build and successful authorized transfer with verified SHA-256. Interruption, cancellation and recovery tests pass. No YouTube-dependent demo.

## 3. M2 — Browser extension and secure desktop handoff (P0)

- [ ] B01: Reuse approved AB Browser Integration source/architecture rather than duplicating its core functions.
- [ ] B02: Define versioned extension-to-desktop request/response schema and compatibility checks.
- [ ] B03: Implement/verify native messaging or tightly authenticated loopback pairing.
- [ ] B04: Block untrusted origins, malicious tabs, arbitrary file paths and unauthorized local commands.
- [ ] B05: Support user-triggered context-menu and normal permitted file-download capture.
- [ ] B06: Expose accurate job status, errors and cancellation to the extension.
- [ ] B07: Minimize host permissions, extension IDs and data collection; add a privacy declaration.
- [ ] B08: Package Chrome MV3 and test install/reload/update in an isolated browser profile.
- [ ] B09: Add security tests for replay, origin confusion, native host discovery, token leakage and CORS abuse.
- [ ] B10: Review Chrome Web Store policy **before** publication; no unauthorized YouTube functionality.

**Gate M2:** Chrome handoff creates one authenticated job, UI progress updates, and malicious page tests fail closed. Package is verified; store listing remains separate.

## 4. M3 — Media processing, subtitles and permitted streams (P1)

- [ ] M01: Safely ingest user-owned local video/audio without automatic cloud upload.
- [ ] M02: Select approved FFmpeg binaries/build flags and ship required notices.
- [ ] M03: Probe metadata and offer only actually supported formats/resolutions.
- [ ] M04: Convert/remux MP4, MKV and WebM; validate playable output.
- [ ] M05: Extract/convert MP3, M4A, FLAC, WAV and Opus.
- [ ] M06: Support audio track/language and SRT/VTT subtitle handling with correct sync.
- [ ] M07: Support trim, merge, compression and simple presets on owned assets.
- [ ] M08: Add bounded batch processing, cancel, timeout, disk quota and temp cleanup.
- [ ] M09: Process authorized non-DRM HLS/DASH fixtures; no DRM or authentication bypass.
- [ ] M10: Add format-specific regression fixtures and playback/integrity checks.
- [ ] M11: Introduce optional rights-reviewed source connectors only after policy/security approval.
- [ ] M12: Report unsupported or protected sources clearly and without speculative workarounds.

**Gate M3:** All supported media fixtures pass; transcodes are bounded, outputs playable, metadata honest and license notices shipped.

## 5. M4 — Website and optional hosted processing (P1)

- [ ] W01: Build a unique accessible product website, download links, feature descriptions and support docs.
- [ ] W02: Decide whether website is marketing-only, local companion UI, or hosted converter (owner approval).
- [ ] W03: Reuse/adapt ReClip only after license/security audit and interface definition.
- [ ] W04: Implement secure upload ingestion for user-owned files if hosted conversion is approved.
- [ ] W05: Restrict direct URL schemes, redirects, DNS and private networks to prevent SSRF.
- [ ] W06: Set per-job time, size, CPU, RAM, disk, bandwidth and concurrency limits.
- [ ] W07: Require appropriate authentication/quotas; protect stored media and downloads.
- [ ] W08: Delete temporary assets by retention policy, with clear privacy notice.
- [ ] W09: Add backend integration tests, abuse tests and cost monitoring.
- [ ] W10: Deploy publicly only with owner approval, legal review and verified infrastructure costs.

**Gate M4:** Secure, accessible web flow with documented retention, observed cost and no open file-fetch proxy.

## 6. M5 — Quality, security and production distribution (P1)

- [ ] P01: Establish GitHub Actions: lint/typecheck/unit/integration for approved components.
- [ ] P02: Add Windows desktop, Chrome, and secondary OS/browser release build matrix.
- [ ] P03: Implement proper versioning, changelog, release notes and rollback strategy.
- [ ] P04: Sign desktop binaries and verify download/update signatures.
- [ ] P05: Generate SBOM, dependency licenses and third-party notices for each package.
- [ ] P06: Test clean install, upgrade, downgrade protections and uninstall on target OS.
- [ ] P07: Perform dependency scanning, secrets scanning, SAST and permission review.
- [ ] P08: Run crash/restart, network outage, storage exhaustion and memory tests.
- [ ] P09: Improve accessibility, keyboard navigation and localization.
- [ ] P10: Create privacy/terms/support/security reporting pages and disclosure process.
- [ ] P11: Publish release artifacts only with permission, packaging checks and user-facing documentation.
- [ ] P12: Collect opt-in usability feedback and correct beta-blocking defects.

**Gate M5:** Reproducible signed release that meets quality, security and legal/distribution standards.

## 7. M6 — Competitive product expansion (P2, AFTER beta)

- [ ] X01: Advanced scheduling, templates, categories and deduplicated download history.
- [ ] X02: API/CLI and opt-in automation with authenticated tokens.
- [ ] X03: Secure plugin SDK, sandbox and permissions model.
- [ ] X04: Advanced archive/media organization and search.
- [ ] X05: Mobile companion and remote management with user pairing and cost controls.
- [ ] X06: Optional local AI transcription/tagging on user-owned media with opt-in processing.
- [ ] X07: Performance comparisons against IDM/4K competitors using fair test methodology.
- [ ] X08: Monetization decision, product analytics, distribution/support operations.

**Gate M6:** Each feature must have demand evidence, explicit business/maintenance cost, a security plan and tests.

## Blocker and handoff protocol

When something is blocked, Codex or a developer must provide:

~~~text
Task ID:
Milestone:
Exact command and environment:
Observed error/output:
Expected result:
Steps already tried:
Security/license impact:
Suggested next action:
Owner decision or credential required?:
~~~

Mark the task **BLOCKED** in the issue or PR. Do not mark blocked work complete; do not bypass permissions or import conflicting code. An unrelated milestone can continue only if it does not depend on the blocked task.

## Progress summary

| Milestone | State | Evidence |
| --- | --- | --- |
| G: governance | PARTIAL | Initial documents exist; product license/name require owner decision |
| M0: repo/architecture/license audit | IN PROGRESS | [Updated audit](M0_AUDIT.md): 17 source candidates triaged, exactly two foundations recommended, both upstream extension builds PASS, 14 fixture tests PASS; Gradle bootstrap, extension types, transitive SBOM and owner decisions remain BLOCKED |
| M1: desktop runtime | NOT STARTED | No application source imported; only a preparatory HTTP fixture exists |
| M2: browser handoff | NOT STARTED | No extension source imported |
| M3: media processing | NOT STARTED | No FFmpeg integration |
| M4: website | NOT STARTED | No website implementation |
| M5: production release | NOT STARTED | Initial fixture-only PR/manual CI exists locally, but no app CI, installers or release pipeline |
| M6: expansion | NOT STARTED | Backlog only |

**Next action:** Unblock A05/A06 with an authorized Gradle 9.8/JDK 25 setup; finish A03 dependency/NOTICE scan and A09 TypeScript repair in an approved upstream fork; verify native protocol security A08/A10. The 17-candidate source triage is documented, but each unchecked audit item still requires its stated full acceptance evidence. Resolve G03–G06 before importing or rewriting desktop/extension source.

<!-- END docs/IMPLEMENTATION_CHECKLIST.md -->


---

# SOURCE 15: docs/M0_AUDIT.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/M0_AUDIT.md  
Blob: `fbd42a033193fa72f7f2fdac08e89c4998560db7`

<!-- BEGIN docs/M0_AUDIT.md -->

# M0 audit log — first engineering pass

**Initial pass:** 13 November 2026. **Follow-up:** 15 November 2026.

**Status:** IN PROGRESS — 17 study repositories triaged at pinned commits, both upstream extension bundles build, local Python fixture tests pass. No user-facing Downloader application has been imported or released.

**Scope:** In-depth initial source audit of AB desktop/extension, targeted source investigation of 15 additional candidates, reproducible build probes, and original test fixture work. Full dependency licensing and third-party binary verification remain open.

This log distinguishes **observed source/build evidence** from proposed feature work. It is intended to let a new developer or Codex agent resume without spending time rebuilding features upstream already supplies.

## Reproducible source pins

| Candidate | Source | Checked-out commit | Declared license | Assessment |
| --- | --- | --- | --- | --- |
| Desktop base | [amir1376/ab-download-manager](https://github.com/amir1376/ab-download-manager) | [68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68](https://github.com/amir1376/ab-download-manager/commit/68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68) | Apache-2.0, root LICENSE | **Strong provisional base**; desktop runtime/test not yet verified |
| Browser companion | [amir1376/ab-download-manager-browser-integration](https://github.com/amir1376/ab-download-manager-browser-integration) | [bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05](https://github.com/amir1376/ab-download-manager-browser-integration/commit/bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05) | Apache-2.0, root LICENSE and package.json | **Builds for Chrome**; security review still needed |

The two directories were shallow-cloned for inspection only. **Neither upstream codebase is committed into the Downloader project.** Brand assets, bundled tools, transitive dependencies and notices require separate review even when a root repository declares Apache-2.0.

## Build evidence

| Check | Environment/command | Observed result |
| --- | --- | --- |
| Extension dependency install | Linux, Node 24.22.1, npm 11.9.0; from browser integration clone: `npm ci --ignore-scripts --no-audit --no-fund` | **PASS**, 535 packages installed from existing lockfile |
| Chrome MV3 extension build | Same clone: `npm run build:chrome` | **PASS**, Vite built 452 modules and emitted `dist/chrome/manifest.json` in 6.87 s |
| Firefox extension build | Same clone: `npm run build:firefox` | **PASS**, Vite built 453 modules and emitted `dist/firefox/manifest.json` in 20.09 s; browser install not tested |
| Standalone TypeScript validation | Same clone: `npx tsc --noEmit` | **FAIL**, exit code 2: missing `~/backend/NativeMessaging`; manifest `author` type mismatch; `webext-bridge`/`type-fest` generic/type errors. Bundling success does **not** imply type correctness. |
| Actual Chrome install and desktop handoff | Not attempted; full desktop application not built | **NOT VERIFIED** |
| Upstream extension automated tests | No `test` script in inspected `package.json` | **NOT AVAILABLE IN PACKAGE SCRIPTS**; add project tests |
| Gradle wrapper bootstrap (desktop prerequisite) | Linux, OpenJDK 21.0.13; in desktop clone: `./gradlew --version` | **BLOCKED**, exit code 1: wrapper requests `https://services.gradle.org/distributions/gradle-9.8.0-bin.zip` but fails `java.net.SocketException: Network is unreachable`. No Gradle desktop compile attempted. |
| Desktop unit/integration tests | Tests not run | **NOT VERIFIED** |

Extension build warnings: deprecated Vite CommonJS API, older Browserslist/caniuse-lite data, several deprecated npm packages, and a `process` module browser-externalization warning. These are not build failures, but should be tracked in dependency maintenance and a clean-browser smoke test. The independent typecheck failure **is** a quality blocker for adopting this code as-is.

## What the sources already provide

| Area | Evidence in pinned source | Reuse decision |
| --- | --- | --- |
| Download engine | Desktop repo has `downloader/core`, `downloader/monitor`, `desktop/app`, `shared/app` modules and Gradle projects | Reuse candidate; validate actual downloading, resume, history and failure handling with controlled fixtures |
| User-configurable concurrency/retries | `shared/app/.../DefaultAppSettings.kt` exposes `maxConcurrentDownloads = 3`, `maxDownloadRetryCount = 3`, thread count and speed-limit controls | Prefer reuse; tests still needed |
| Desktop HTTP API | `integration/server/.../Integration.kt` binds to `127.0.0.1`; `Endpoints.kt` has `POST /add`, `POST /ping`, `GET /queues` | Adapt and harden, not greenfield |
| Native browser messaging | Extension `src/backend/nativemessaging/NativeMessagingTransport.ts` and `src/backend/Backend.ts` | Reuse, test host registration, origin validation and handoff |
| Fallback HTTP extension integration | Extension `src/backend/HttpApi.ts` invokes localhost API and can attach configured key | Audit auth and origin handling; require secure pairing |
| Chrome Manifest V3 | Extension `src/manifest/manifest.chrome.ts`; emitted `dist/chrome/manifest.json` has `manifest_version: 3` | Existing MV3 foundation; packaging and browser store review pending |
| HLS link interception | Extension `src/linkgrabber/DownloadLinkInterceptor.ts` matches `.m3u8` patterns and labels media type `hls` | **Do not build a second HLS detector yet**; test permitted non-DRM fixtures |
| Download context menus | Extension source and README describe user-initiated download actions | Prefer reuse; manual UX test pending |
| Site-specific extractors | No broad verified site-support audit in this pass | No promised support; enforce rights/platform-policy review first |

## Security findings — must address before release

**SEC-001 (high-priority design blocker): the upstream loopback API permits unauthenticated requests by default.** In `shared/app/src/commonMain/kotlin/com/abdownloadmanager/shared/storage/appsettings/DefaultAppSettings.kt`, `apiEnabled` defaults to `true` while `apiAuthEnabled` defaults to `false`. The server `integration/server/.../Endpoints.kt` explicitly skips API-key validation on some routes when `apiKeyEnforced` is false; the app binds to loopback `127.0.0.1`. For **our proposed distribution**, make authentication and origin/pairing protections mandatory for downloaded jobs, test malicious webpage requests, and consider disabling HTTP fallback if safe native messaging suffices. Loopback binding alone is not authentication. No exploit is asserted or tested here.

**SEC-002 (review before public Chrome distribution): the upstream MV3 manifest requests broad access.** The built extension contains `host_permissions: ["*://*/*"]`, matches all HTTP(S) pages with its content script, and asks for `cookies`, `webRequest`, `downloads`, `nativeMessaging`, `tabs`, `notifications`, `storage` and `contextMenus`. Establish feature-to-permission justification, reduce scope where feasible, test data minimization and verify store compliance. Do not submit the unreviewed upstream package as our product.

**SEC-003 (user media and URL policy):** Header/cookie forwarding, redirects, private-network requests, DRM, protected sources and platform terms must be reviewed. Only test with content the owner controls or has explicit permission to process. No YouTube extraction demo is a release gate.

## Reproducible blocker handoffs

### BLK-001: Gradle 9.8 bootstrap network restriction

~~~text
Task ID: A05/A06
Environment: Linux / OpenJDK 21.0.13 / pinned desktop clone
Command: ./gradlew --version
Observed: Downloading https://services.gradle.org/distributions/gradle-9.8.0-bin.zip
          java.net.SocketException: Network is unreachable (exit 1)
Expected: Gradle wrapper starts, allowing targeted desktop compile/tests.
Tried: Confirmed source checkout, JDK version and Gradle distribution URL.
Suggested fix for Codex: use an environment where that download is explicitly allowed,
  or provision the exact verified Gradle 9.8 distribution into a local cache;
  then run narrow desktop tests/build. Do not disable TLS verification or use
  arbitrary mirrors and do not claim a desktop build passed.
~~~

### BLK-002: Upstream extension does not typecheck

~~~text
Task ID: A09 / future B01
Environment: Linux / Node 24.22.1 / npm 11.9.0 / pinned extension clone
Commands: npm ci --ignore-scripts --no-audit --no-fund
          npm run build:chrome
          npm run build:firefox
          npx tsc --noEmit
Observed: Both bundler builds pass, but tsc exits 2 with:
  TS2307 Cannot find module '~/backend/NativeMessaging' (src/backend/BackendProvider.ts)
  TS2322 manifest 'author' type incompatible (chrome and Firefox)
  TS2344/TS2305 type errors in node_modules/webext-bridge (incl. type-fest Jsonify)
Expected: Both builds and a separate TypeScript typecheck pass without masking errors.
Suggested fix for Codex: audit actual source import and compatible dependency/type
  versions; write targeted changes in an approved fork after owner license decision.
  Do not blanket-disable typechecking or silently skip library checking as a release fix.
~~~

## Reuse versus build — early matrix

| Component | Current recommendation | Additional evidence/gap |
| --- | --- | --- |
| Core download engine and desktop UI | **Reuse/adapt AB candidate** | Desktop compile, tests, dependency license scan, Windows packaging |
| Chrome browser bridge | **Reuse/adapt AB extension** | Security hardening, versioned handoff, browser installation test |
| Basic HLS detection | **Reuse existing extension logic first** | Verify with local, authorized HLS fixtures; no DRM bypass |
| Queue/schedule/history | **Validate source features before writing** | Controlled fixtures, restart recovery, UX inspection |
| Local conversion/transcoding | **Integrate FFmpeg later** | Binary/license flags, codec availability, sandboxing |
| Website | **Build original landing site or audit ReClip after scope decision** | Marketing-only vs local companion vs hosted service changes security and cost substantially |
| Optional source connectors | **Defer** | Permissions, site policies, package licenses and tests |
| Custom security tests and direct-download fixtures | **Build original tests for our acceptance gates** | M1/M2 work after base approval |

## Next ordered audit tasks

1. **A03/A05/A06:** resolve BLK-001 in a permitted environment, audit desktop dependency notices and run a narrowly scoped Gradle desktop build/tests with exact failure logs; do not provision paid infrastructure.
2. **A08/A10:** audit message validation and localhost API threat model; verify extension handoff against a locally running app with user-controlled file URLs.
3. **A11–A17:** pin/audit ReClip, MediaGrabber, HLS Downloader, FFmpeg, yt-dlp and alternative feature references. Do not mix incompatible-licensed source.
4. **A18–A21:** implement legal synthetic HTTP fixtures, establish baseline benchmarks and revise the feature matrix and architecture decision based on results.
5. Request owner decisions **G03–G06** and **G08** before any code import, public release, hosted deployment or paid workload.

## Copyable Codex continuation instruction

> Read AGENTS.md, docs/IMPLEMENTATION_CHECKLIST.md and docs/M0_AUDIT.md. Continue from A03/A05/A06 using the two pinned upstream SHAs above. Reproduce and fix BLK-001 (Gradle network/toolchain) in an authorized environment and investigate BLK-002 (extension TypeScript errors). Do not clone arbitrary HEAD or import upstream source into this project until the owner decides branding/license/base. First report exact desktop build/test commands, dependency-license risks and failures. Flag authenticated loopback/native pairing as mandatory. Update checked items only with evidence. No paid cloud/GPU or public deployments, and no unsupported YouTube/DRM functionality.

**Current product status:** The repo contains documentation and audit evidence, not a runnable Downloader application. Chrome extension build success is evidence about the **upstream candidate**, not our product.

## First original project code: local download fixture

The repository now includes [tools/fixtures/http_fixture.py](../tools/fixtures/http_fixture.py), a dependency-free loopback server with deterministic 16 KiB content, Range/206, unsatisfiable 416, a Range-disabled path and SHA-256 validation; see its [usage guide](../tools/fixtures/README.md). It is **test infrastructure**, not a downloader or imported third-party implementation.

**Initial baseline (13 November):** `python3 -m unittest discover -s tests -v` — **9 tests PASS** in 0.597 seconds on Linux. The fixture has since been extended; see the follow-up evidence below. Neither the fixture nor an upstream extension bundle is a usable desktop application.

## 15 November 2026 follow-up: source triage, security and original code

The [17-repository matrix](REUSE_DECISION_MATRIX.md) records an exact observed default-branch commit, build/source locations, root license declarations and an individual decision for each candidate. The two **conditional** primary recommendations remain AB Download Manager (68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68) and its AB Browser Integration companion (bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05). There is **no authorization or verified app build** yet. The [build-start plan](BUILD_START_PLAN.md) defines the incremental architecture, M0/M1/M2 gates and exact approvals needed.

### Reproducible follow-up commands and outcomes

| Location | Command / source observation | Result |
| --- | --- | --- |
| Downloader repo | `python3 -m unittest discover -s tests -v` | **PASS: 14/14** deterministic fixture tests, approximately 1.12 s, Python 3.12.14 on Linux |
| Downloader repo | `python3 -m compileall -q tools tests` | **PASS** |
| AB extension pinned checkout | `npm ci --ignore-scripts --no-audit --no-fund` | **PASS** with lockfile; 535 installed packages |
| AB extension pinned checkout | `npm run build:chrome` | **PASS**, Vite 452 modules, approximately 2.64 s |
| AB extension pinned checkout | `npm run build:firefox` | **PASS**, Vite 453 modules, approximately 2.64 s |
| AB extension pinned checkout | `./node_modules/.bin/tsc --noEmit` | **FAIL**, TS2307 missing NativeMessaging import, TS2322 manifest author mismatch, TS generic/type-fest issues (issue #5) |
| AB extension pinned checkout | `npm audit --omit=dev --audit-level=moderate --json` | **FAIL (exit 1): two high advisories**, lodash GHSA-r5fr-rjxr-66jc and zod range <4.6.6, from npm audit output; assess affected runtime paths and lockfile remediation in an approved fork. An advisory is not proof of exploitability. |
| AB desktop pinned checkout | `./gradlew --version` | **BLOCKED**: official Gradle 9.8 binary download encounters `java.net.SocketException: Network is unreachable` (issue #4); no desktop compile/test run |
| AB desktop pinned source | gradle/wrapper/gradle-wrapper.properties and gradle.properties | Wrapper **9.8.0**, `jvm.toolchain=25`; current probe JDK 21 does not satisfy requested toolchain. Validate available JDK 25 distribution in permitted environment. |

The original fixture now supports optional TLS with a caller-supplied certificate, verified HTTPS requests (including rejection of an untrusted certificate), matching/mismatched If-Range ETags, relative redirect, intentionally truncated response, and denial of excessively long Range offsets. It binds only to 127.0.0.1 and does not fetch user-provided URLs or disable certificate validation. A small [fixture-only GitHub Actions workflow](../.github/workflows/fixture-tests.yml) is added with pinned checkout/setup-python actions and read-only permissions; **the workflow is not yet proven by an observed hosted GitHub run**. M1 runtime integration and benchmarks remain undone.

### Security decisions to preserve

- **High:** upstream AB `apiEnabled=true` but `apiAuthEnabled=false` by default; integration `/add` may skip key enforcement and can feed silent-import flow. The local bridge must authenticate and authorize job-creating requests; loopback binding does not suffice. Native messaging is preferred for the first slice (issue #6).
- **High:** extension's broad `*://*/*`, cookies, webRequest, nativeMessaging and content-script access require privacy minimization and negative-origin tests before packaging.
- **Supply chain:** two high production-dependency advisories appeared in a pinned upstream npm audit. Preserve evidence and evaluate targeted upgrades without bypassing TypeScript checks.
- **Website:** ReClip's Flask backend runs submitted URLs via yt-dlp and keeps jobs in process memory. No adequate default hosted SSRF, authentication, quota or persistence guarantees were evident in inspected code; **reference its UX only**, pending an original safe design.
- **Licensing:** MediaGrabber's bundled Windows FFmpeg is identified as GPLv3 in its third-party notices; FFmpeg's LGPL/GPL behavior is build dependent. Gopeed, aria2, Cobalt, MeTube, Pinchflat, Tube Archivist and other GPL/AGPL examples are not silently importable into an unlicensed product.
- **Policy:** support user-owned/expressly permitted downloads, review browser-store requirements, and reject DRM/protected source circumvention. Do not promise general YouTube integration.

**Open work:** exact transitive SBOM and NOTICE inventory, full desktop Java 25 + Gradle boot/build/tests, extension typecheck repair in an owner-approved fork, authentic Chrome/native handoff, user decisions G03–G06/G08, and platform policy review. All remain unchecked in the [implementation checklist](IMPLEMENTATION_CHECKLIST.md).

<!-- END docs/M0_AUDIT.md -->


---

# SOURCE 16: docs/BUILD_START_PLAN.md

Original: https://github.com/kawakaze430/Downloader-/blob/main/docs/BUILD_START_PLAN.md  
Blob: `ea717ff6d87dd38ee455f38f80f81beeb82f4209`

<!-- BEGIN docs/BUILD_START_PLAN.md -->

# Build-start plan — verified M0 foundation, then M1 and M2

**Audit date:** 15 November 2026. **Status:** M0 in progress, not yet approved for importing third-party application code.

## 1. What actually exists in our repository

The inspected local checkout initially pointed at main commit **3509d5158630416b656508e2fe35a57265148752** with a clean worktree. No integrated desktop application, extension, media processor or website exists in this repository. Files consist of project documents, issue/PR templates, and an original Python HTTP fixture and test suite. This task adds **original fixture functionality and a small test workflow**, not an application release.

| Area | Existing state | Direct evidence |
| --- | --- | --- |
| Product plan/feature inventory | Detailed but proposed | README, PRODUCT_VISION, FEATURE_MATRIX, ROADMAP |
| License/brand/platform | Owner decisions missing | DECISIONS ADR-009/010/011 and checklist G03–G06 |
| Original runtime source | Python loopback HTTP fixture only | tools/fixtures/http_fixture.py |
| Automated tests | Baseline 9 tests passed before edits; expanded to **14 passing** | python3 -m unittest discover -s tests -v |
| HTTPS test route | Added to fixture, using caller-supplied cert/key and ephemeral cert generation in tests | create_server(... certfile, keyfile), HTTPSFixtureTests |
| Controlled failure handling | Added If-Range, abrupt EOF and relative redirect | fixture endpoints/tests |
| CI | New minimal Python fixture workflow, not full app CI | .github/workflows/fixture-tests.yml |
| Desktop executable / Chrome-to-app demonstration | **Not implemented** | No app source imported; no Windows install evidence |
| Cloud/service/backend | **Not implemented** | No deployment, credentials or paid resources |

### Current repository tree (conceptual)

~~~
.github/
  ISSUE_TEMPLATE/
  PULL_REQUEST_TEMPLATE.md
  workflows/fixture-tests.yml       # new, tests only
docs/
  ... existing vision, security, audit, checklist ...
  REUSE_DECISION_MATRIX.md           # 17-candidate evidence and selections
  BUILD_START_PLAN.md                # this document
tools/
  fixtures/http_fixture.py          # existing and extended original code
  fixtures/README.md
tests/
  test_http_fixture.py              # original tests, extended
README.md  AGENTS.md  CONTRIBUTING.md  SECURITY.md
~~~

Do not create empty apps/desktop, apps/extension, packages or a new job engine solely to imitate a future monorepo. Preserve the selected AB layout in an **approved fork or pinned subtree** when adoption is authorized. We can place it under an app-level boundary later without renaming every Kotlin module.

## 2. Foundation recommendation

Select exactly two primary upstream projects, conditionally:

1. [AB Download Manager](https://github.com/amir1376/ab-download-manager) at **68f0ea6e8ba5cbeb02c92342cd3bf7475378ae68**, for Kotlin desktop engine, queue, persistence, UI and native host.
2. [AB Browser Integration](https://github.com/amir1376/ab-download-manager-browser-integration) at **bf6811dd8a3ee2b47b94d70e13c2c7e70be7be05**, for MV3 Chrome, Firefox, existing request protocol and browser UX.

Why: a designed integration already exists, the engine has implementation evidence (not merely README claims), and both roots declare Apache-2.0. The alternative Parabolic uses C#/.NET 10 + yt-dlp workflows rather than AB's general direct-file engine; Gopeed uses Go/Flutter under GPL-3.0; Motrix uses Electron/aria2 with extra licensing/runtime concerns. ReClip's current Flask backend accepts arbitrary URLs and is unsuitable as a public service without major SSRF/auth/resource controls.

Evidence for all 17, commit pins and reasons are in [REUSE_DECISION_MATRIX.md](REUSE_DECISION_MATRIX.md). This selection is **not an authorization to copy source code**.

## 3. Proposed desktop-first architecture

~~~mermaid
flowchart TD
    B["Chrome user action"] --> E["Extension background"]
    E --> N["Native messaging host"]
    N --> C["Desktop integration controller"]
    U["Desktop UI"] --> C
    C --> J["AB job/queue engine"]
    J --> D["Persisted jobs and download parts"]
    J --> F["User-approved local files"]
    F --> V["Integrity checks and safe finalize"]
~~~

Existing native messaging manifests already restrict the Chrome extension origin and Firefox extension ID. **Keep that boundary.** The desktop integration handler currently supports both GUI import and a separately authenticated headless API. A browser-originated request must be treated as untrusted input even when native messaging authenticated the extension.

**Local HTTP bridge:** The upstream server binds 127.0.0.1 and skips authentication when its optional auth setting is disabled; default is disabled. The extension currently prioritizes HTTP fallback unless native messaging is already connected. For our intended release, require authenticated, version-aware pairing for *every* job-creating route, and prefer native messaging for the first slice. Either remove the unauthenticated HTTP fallback or make it independently secure. See issue #6.

**Not in the first slice:** hosted conversion, Redis, PostgreSQL, arbitrary remote job control, AI features, public source extractors and billing. None is needed to prove local file reliability.

### Proposed job lifecycle

~~~mermaid
stateDiagram-v2
  [*] --> Queued
  Queued --> Resolving
  Resolving --> Downloading
  Downloading --> Paused
  Paused --> Queued
  Downloading --> Verifying
  Verifying --> Complete
  Resolving --> Failed
  Downloading --> Failed
  Verifying --> Failed
  Failed --> Queued: bounded retry
  Queued --> Cancelled
  Downloading --> Cancelled
  Complete --> [*]
  Cancelled --> [*]
~~~

**Compatibility rule:** AB already has download/job/queue model classes and a native messaging request protocol. The first implementation should **adapt its existing shapes**, not create a competing engine or prematurely assert that a newly imagined JSON format is implemented. Before B02, define the smallest version/feature negotiation and compatibility tests around the existing message shapes.

**New bridge validation rules to implement after adoption approval:**

- Only the pinned, user-installed extension ID may invoke native actions; parse a bounded JSON message, reject unknown actions and schema-invalid inputs.
- Require user intent, preferably a GUI confirmation for the first authorized direct URL. Reject silently submitted jobs absent an explicit opt-in and authenticated pairing.
- Restrict HTTP fallback by strong tokens, origin/Host checking, short timeouts, replay controls and no wildcard CORS; consider disabling it entirely for beta.
- Validate allowed URL schemes and redirects, avoid private/loopback fetches when proxying arbitrary remote URLs, and never accept an untrusted absolute destination path.
- Sanitize filenames, handle collisions and disk-full/permissions errors, cap concurrency and retries, and never log cookies, Authorization headers or private links.
- Persist job state before acknowledging creation; success requires complete/verified output rather than only an HTTP 200 response.

## 4. M0 exit work — explicit order

| Order | Task / checklist | Acceptance and evidence | Current status |
| --- | --- | --- | --- |
| 1 | Check upstream provenance A01/A07 | Exact commit and declared root license; verify no surprise branch switch | **PASS** for both pins |
| 2 | Inspect engine/queue/bridge source A02/A04/A08 | Reuse map with code paths; separately identify tests not yet run | **PARTIAL**, source evidence found, runtime unverified |
| 3 | Desktop JDK/Gradle setup A05 | Gradle 9.8 boots; JDK 25 toolchain available; exact desktop task succeeds | **BLOCKED** on official release-asset egress, issue #4 |
| 4 | Desktop tests A06 | Targeted Kotlin/desktop tests run with results; controlled file transfer | **NOT RUN**; upstream test suites to enumerate |
| 5 | Extension bundling and types A09 | npm ci, Chrome/Firefox builds, tsc all pass | **PARTIAL**: builds PASS, typecheck FAIL, issue #5 |
| 6 | Bridge threat model A08/A10 | Native protocol and HTTP rules documented; negative test plan | **PARTIAL**, design/issue #6, no runtime handoff |
| 7 | 15 alternatives A11–A17 | Source triage, exact branch pins, root licenses and dependency risk list | **PARTIAL**, documented in reuse matrix; no full SBOM/build |
| 8 | Fixture-based benchmark A18 | Controlled HTTPS/Range/error cases and actual app throughput/integrity data | **PARTIAL**: fixture tests PASS, app not available |
| 9 | Reuse matrix and decision A19–A21 | One pair recommended; owner explicitly approves decisions | **RECOMMENDED**, owner signoff pending |

Desktop bootstrap: with Linux/OpenJDK 21, running ./gradlew --version in the pinned upstream tree still fails with java.net.SocketException: Network is unreachable. Direct curl reaches services.gradle.org but its Gradle 9.8 binary redirect to GitHub release assets is not permitted from this environment. Do not lower TLS integrity, use untrusted mirrors, or mark the desktop build as passing. The root gradle.properties separately specifies jvm.toolchain=25, so **JDK 25 is another prerequisite to verify**.

Extension reproducible checks:

~~~sh
npm ci --ignore-scripts --no-audit --no-fund
npm run build:chrome
npm run build:firefox
./node_modules/.bin/tsc --noEmit
~~~

The first three **PASS** with the pinned lockfile. TypeScript **FAILS**: src/backend/BackendProvider.ts references a missing NativeMessaging module; Manifest author type mismatch; webext-bridge declaration errors including its type-fest Jsonify import. The branch builds are **not proof of type safety or runtime compatibility**. Fix only in an owner-approved fork, with targeted, non-suppressive changes.

## 5. M1 smallest working desktop slice — after owner approval

1. Select a fork/subtree strategy that retains AB upstream provenance and Apache-2.0 notices. Record original commit and any changes.
2. Reproduce a clean desktop build (Linux first; Windows 11 target) with exact JDK/Gradle/Android-disabled configuration. Keep existing modules and DB implementation.
3. Use our Python fixture's local HTTPS endpoint with a trusted temporary test cert. The app must download a known SHA-256 file to a user-approved directory.
4. Test actual progress (bytes/speed), pause/resume (Range+If-Range), retry after truncated response, cancellation, 302 redirect and no-range safe restart.
5. Test filename/path validation, collision, disk-full behavior, finite retries and failure messages.
6. Kill/restart app and verify persisted partial state and safe output finalization; compare SHA-256.
7. Add focused integration tests for each scenario, document clean-build commands and update the checklist only where behavior passed.

**M1 gate:** reproducible Windows 11 binary, authorized HTTPS file download with real progress, verified output checksum, safe cancellation/recovery and no required internet video source.

## 6. M2 smallest secure browser slice — after M1 and owner approval

1. Fork the pinned AB extension without reimplementing its existing popup/context-menu code.
2. Fix TypeScript errors, package MV3 Chrome and document exact compatible extension/desktop versions.
3. Pair the browser to the native host with registered extension ID, bounded messages and explicit user action.
4. Send one permitted HTTPS fixture job and observe desktop acknowledgement and progress.
5. Reject unknown extension IDs, forged origins, unauthenticated localhost HTTP requests and malicious paths.
6. Review broad host/cookie/webRequest permissions; minimize them without falsely claiming those features work.
7. Add Chrome install/reload/update smoke test on Windows 11. Do not claim Chrome Web Store eligibility without a policy review.

**M2 gate:** one user action creates exactly one authenticated desktop job; negative tests fail closed; no unauthorized YouTube or protected media features.

## 7. Owner approvals required before imports/releases

1. **G03:** public product name, identity and trademark/logo policy.
2. **G04:** original-code license (open-source or proprietary), distribution and contributor terms.
3. **G05:** first target Windows 11 + Chrome (recommended); secondary OS/browser priorities.
4. **G06:** scope: authorized direct downloads first, media tools later, optional web service later.
5. **G08:** CI/cloud usage budget and whether public Actions PR checks may run.
6. **Gate M0:** approve the exact two upstream commits and Apache-2.0/third-party notice strategy after dependency audit.
7. **ADR-014:** content source/rights policy and public store/distribution constraints.

Until those decisions are recorded, do not import the full upstream app, claim ownership of third-party code, publish a product, or create paid resources.

## 8. Cost-conscious continuation prompt

> Continue M0 for Downloader using docs/REUSE_DECISION_MATRIX.md, docs/BUILD_START_PLAN.md and docs/M0_AUDIT.md. Resolve issue #4 only via an environment with permitted verified Gradle 9.8 and JDK 25 binaries; otherwise record the exact blocker. Resolve issue #5 in an approved isolated upstream fork, without hiding TS errors. Harden the protocol described in issue #6 only after owner decisions. Preserve the current original Python fixture, run its 14 tests, and review the manual/PR fixture CI. Do not deploy or import GPL/AGPL code, and do not claim M1/M2 done without a real Windows + Chrome integration test.

**Definition of progress:** new code, tests, measured results and durable audit evidence. A passing upstream extension build alone does not make our Downloader application runnable.

<!-- END docs/BUILD_START_PLAN.md -->
