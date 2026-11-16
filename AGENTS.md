# Instructions for coding agents and AI contributors

> **Session continuity:** When a new conversation lacks history, use [docs/context-archive/README.md](docs/context-archive/README.md) and the concise [resume prompt](docs/context-archive/RESUME_PROMPT.md). The [master archive](docs/context-archive/PROJECT_MASTER_CONTEXT.md) is a dated historical snapshot, not a replacement for live files and CI; read it selectively to conserve context and credits.

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
