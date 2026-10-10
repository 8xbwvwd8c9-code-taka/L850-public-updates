# L850 Public Updates — CURRENT

```text
DATE=2026-10-10
STATUS=PLAN_ONLY_SCAFFOLD
PUBLIC_REPO=8xbwvwd8c9-code-taka/L850-public-updates
DEFAULT_BRANCH=main
SOURCE_CODE_IN_PUBLIC_REPO=NO
MANIFEST_PATH=stable/manifest.json
MANIFEST_PUBLISHED=false
CURRENT_RELEASE=NONE
DRIVE_PATCH_VERIFIED=NO
PUBLIC_MANIFEST_URL=https://raw.githubusercontent.com/8xbwvwd8c9-code-taka/L850-public-updates/main/stable/manifest.json
API_TOKEN_REQUIRED_BY_PLAYER=NO
GOOGLE_DRIVE_API_REQUIRED_BY_PLAYER=NO
LAUNCHER_NOTICE_IMPLEMENTED=NO
AUTO_DOWNLOAD=PAUSED
AUTO_EAT=PAUSED
GAME_LAUNCH_AND_HELPER_MODIFICATIONS=NO
AGENT_NEXT=FOLLOW_DOCS_AGENT_TASK_PLANNING_ONLY
```

## Scope

This repository is **PUBLIC** and contains only safe metadata/documents. It is not the development source tree. No game IPs, private code, binaries, API credentials, links to internal working artifacts or unreviewed patch content are permitted.

The existing Google Drive manual-patch download workflow is unchanged. The stable manifest is a deliberately **unpublished** sentinel. No verified release ZIP, version, size or hash is available yet; do **not** set `published=true` until real bytes and a public manual-download link pass operator approval.

## Precedence and agent entry

Read `README.md` → `docs/CURRENT.md` → `docs/AGENT_TASK.md` → `docs/PUBLISHING.md` → `schemas/manifest.schema.json`. Do not borrow unpublished values from examples and do not promote documentation PASS into a shipped Rust client feature.

## Future human approval gates

- Separate, explicit approval needed to publish a verified release (`published=true`).
- Separate, explicit approval needed to implement the Rust notification UI.
- Automatic download, installation, `eat.exe`, game-process actions and cloud infrastructure remain **out of scope**.

## Invariants

Published update means a new `release_id`, nonzero actual bytes, actual SHA256 of final ZIP, valid public Drive URL, schema validation and review. For a disabled manifest or network outage, the future Launcher must continue the original launch path and must not falsely claim latest version.
