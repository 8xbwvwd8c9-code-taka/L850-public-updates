# L850 Public Updates — Worklog

## 2026-10-10 — Initial public metadata plan scaffold

- Existing public empty repo `L850-public-updates` initialized on `main` with documentation and a `published=false` stable manifest sentinel.
- Manifest JSON schema and an explicitly invalid sample define future publishing. A release remains `NONE`; no actual ZIP size, digest or Google Drive public share was certified.
- Existing Drive manual download stays unchanged; no player EXE GitHub/Google token, no automatic installation, no updater code or new cloud service.
- Operator procedure and scoped agent task documented. This is **PLANNING**, not a live client/build/installer PASS.


## 2026-10-10 — WebView2 fetch/IPC design correction

- Scoped proposal added: `docs/WEBVIEW2_NOTICE_DESIGN_20261010.md`; Windows WebView2/Wry `with_html()` null Origin requires explicit GitHub Raw CORS verification.
- User's suggestion to use JS `fetch()` and existing IPC conditionally accepted in DESIGN only; WinHTTP read-only OS fallback if fetch blocked. No new Rust network dependency approved.
- Separate update indicators GREEN/RED/UNKNOWN/UNPUBLISHED from game-launch readiness; `CheckFailed=UNKNOWN`, while the preexisting launch eligibility remains unchanged. Manual Google Drive link only from approved sources; distinguish Check updates from Open download behavior.
- `STATUS=PLANNING_ONLY_PAUSED`; `PUBLIC_MANIFEST_PUBLISHED=FALSE`; `RUST_SOURCE_TOUCHED=NO`; `WIN_X86_CORS_PASS=NOT_PROVEN`.
