# Long-chat continuity archive — start here

Prepared: **16 November 2026**. Canonical repository: https://github.com/kawakaze430/Downloader-

This folder is designed to solve the lost-context problem when ChatGPT/Codex or a human contributor joins the project without access to the original long conversation.

## Read/download

| Document | Purpose |
| --- | --- |
| [PROJECT_MASTER_CONTEXT.md](PROJECT_MASTER_CONTEXT.md) | **Complete historical knowledge-base archive**: 23 source documents reproduced in full, feature tables/charts, repo research, reference links, decisions, architecture and history |
| [PROJECT_MASTER_CONTEXT.rtf](PROJECT_MASTER_CONTEXT.rtf) | **Microsoft Word-compatible document** with the same snapshot; open/save as .docx in Word if desired |
| [CHAT_HISTORY.md](CHAT_HISTORY.md) | Chronological reconstruction of all 12 substantive stages of the user–assistant conversation; decisions, outcomes and caveats |
| [RESUME_PROMPT.md](RESUME_PROMPT.md) | Small prompt for starting a fresh ChatGPT/Codex session |
| [ALL_LINKS.txt](ALL_LINKS.txt) | 76 unique URLs collected from the archive |
| [SOURCE_SNAPSHOT_PART_1.md](SOURCE_SNAPSHOT_PART_1.md) | Exact source documentation snapshot (part 1) |
| [SOURCE_SNAPSHOT_PART_2.md](SOURCE_SNAPSHOT_PART_2.md) | Exact source documentation snapshot (part 2) |
| [SOURCE_SNAPSHOT_PART_3.md](SOURCE_SNAPSHOT_PART_3.md) | Exact source documentation snapshot (part 3) |

## Fast restart procedure

1. In a new ChatGPT or Codex task, share the canonical repo and [RESUME_PROMPT.md](RESUME_PROMPT.md).
2. Ask the agent to read live `AGENTS.md`, `docs/DECISIONS.md`, `docs/LEGAL_SECURITY.md`, `docs/IMPLEMENTATION_CHECKLIST.md`, `docs/M0_AUDIT.md`.
3. Use `PROJECT_MASTER_CONTEXT.md` only when historical context or a referenced research source is needed; **do not load the entire 183K-character master on every small change**.
4. Refresh live branches, PRs, issues, dependencies, licenses, releases and CI. Historical observations are not live evidence.
5. Add future decisions as small updates to the live source docs, then periodically regenerate the archive (it is a snapshot, not a synchronized mirror).

## Accuracy and safety

- This archive is a **reconstructed conversation record**, not a byte-for-byte conversation export. An exact ChatGPT transcript would need to be exported separately.
- The original `Create Video Downloader Extension.txt` attachment was detected but yielded no readable text. Its content is **not included**.
- The historical repo identity `rejoansardar620-lang/Downloader-` appears in source snapshots; current canonical repo is `kawakaze430/Downloader-`.
- The two AB candidates are research recommendations, **not imported first-party production code**.
- The project may be public. Never upload API keys, personal secrets, credentials, private logs or copyrighted media samples.
- The Word-compatible file is **RTF**, not a native .docx; both Word and LibreOffice can open it.
