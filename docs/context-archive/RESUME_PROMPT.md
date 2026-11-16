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
