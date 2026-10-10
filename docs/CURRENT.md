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


## 2026-10-10 — WebView2 notification design review

```text
SCOPED_DESIGN=docs/WEBVIEW2_NOTICE_DESIGN_20261010.md
WEBVIEW_FETCH=CONDITIONAL_PENDING_WINDOWS_X86_CORS_TEST
CURRENT_WRY_WITH_HTML_ORIGIN=null
PUBLIC_GITHUB_RAW_CORS=NOT_VERIFIED
IPC_REUSE=EXISTING_WRY_WITH_IPC_HANDLER
NETWORK_FAILURE_UPDATE_LIGHT=UNKNOWN_GRAY_AMBER_NOT_GREEN
GAME_LAUNCH_GATE=INDEPENDENT_UNCHANGED
MANUAL_DRIVE_BUTTON=APPROVED_URL_ONLY
PUBLIC_MANIFEST=UNPUBLISHED
NOTICE_IMPLEMENTATION=PAUSED
```

The user suggested `WebView2 fetch()` instead of an additional Rust HTTP dependency and a manual Google Drive button. This is acceptable conditionally; the actual private Rust Wry GUI uses `with_html()` (a `null`-origin page) and existing IPC. Fetching GitHub Raw JSON through WebView2 requires a real Windows CORS check; never use `no-cors` to work around readable content. Keep the existing game launch readiness signal distinct from the future update indicator: failed checking is UNKNOWN, not green. Full design and agent acceptance requirements are in `docs/WEBVIEW2_NOTICE_DESIGN_20261010.md`. No source, release, deployment or automatic patch changes authorized.


## 2026-10-10 — Full V0.3 pasted plan: PARTIAL_PASS static review

Full V0.3 text reviewed, including URL validator Rust sample, message flow and schema. Current plan retains `PLAN_ONLY`, `published=false`, and `AUTO_EAT=PAUSED`. Engineering corrections and agent task: `docs/V03_FULL_ARCHITECTURE_REVIEW_20261010.md`. **Source checks required**: Rust must parse and validate URI components (not only string prefix), optional `release_notes` and `mandatory=false` must match public schema, `release_id` must not be lexically ordered as time, and test counts must be taken from current HEAD. Windows WebView2 CORS test remains unexecuted. No source change or live release.


## 2026-10-10 — V0.4 architecture conditional PASS (PLAN ONLY)

Canonical scoped adjudication: `docs/V04_ARCHITECTURE_ADJUDICATION_20261010.md`. Three corrections: C1 strict Drive path validation without filtering empty segments/ambiguous query; C2 local installed receipt missing means `InstallStatusUnknown`, not proven red/newer; C3 all update states retain independent existing game launch/security gates. Public manifest remains `published=false, release=null`. Rust GUI/updater implementation, Windows x86 CORS runtime validation, releases and eat.exe remain NOT DONE / PAUSED.


## 2026-10-10 — V0.5 approved design specification (PLAN ONLY)

The newest spec is `docs/V05_APPROVED_SPEC_20261010.md`. `SPEC_STATUS=APPROVED_WITH_IMPLEMENTATION_NOTES`; strict URL validation, distinct unknown installed status, preservation of launch gates, dynamic tests. `published=false`, `release=null` remains; x86 CORS OPEN; coding needs separate user authorization. V0.4 is historical.
