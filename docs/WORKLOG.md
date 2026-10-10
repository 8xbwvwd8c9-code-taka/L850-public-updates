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


## 2026-10-10 — Full V0.3 plan static review

- User pasted complete V0.3 architecture text; recorded conditional PASS with explicit corrections in `docs/V03_FULL_ARCHITECTURE_REVIEW_20261010.md`.
- Findings: Rust URL prefix validator not sufficient, public schema optional `release_notes` conflict with required Rust String, `mandatory=false` enforcement, version ordering ambiguity, CORS/WinHTTP qualification and live test count evidence.
- `PLANNING_ONLY=YES; UPDATE_NOTICE_NOT_IMPLEMENTED; STABLE_MANIFEST_PUBLISHED=FALSE; AUTO_DOWNLOAD_AND_EAT=PAUSED; GAME_LAUNCH_UNCHANGED`.


## 2026-10-10 — V0.4 pasted plan review

- V0.4 architecture assessed `CONDITIONAL_PASS`; full file `docs/V04_ARCHITECTURE_ADJUDICATION_20261010.md`.
- Closed planning guidance for C1 strict URL path/encoding/query, C2 missing installed receipt cannot be treated as newer, C3 update check must not override existing launch guards; actual CORS and dynamic tests NOT_RUN.
- No changes to shipped Rust, private game source, public manifest, Drive ZIP, auto-download/eat or runtime.
