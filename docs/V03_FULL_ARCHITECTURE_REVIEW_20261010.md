# L850 V0.3 Full pasted architecture review — engineering corrections (PLAN ONLY)

```text
DATE=2026-10-10
STATUS=PARTIAL_PASS
MODE=PLAN_ONLY
PATCH_RELEASE=UNPUBLISHED
RUST_NOTICE_IMPLEMENTATION=PAUSED
WEBVIEW2_CORS_WINDOWS_X86=UNTESTED
SOURCE_CHANGES=NONE
```

## Scope

Review of user's fully pasted L850 public-update notice V0.3 plan, not of a claimed independently opened Antigravity local file. Baseline: private Rust launcher `gui.rs` uses Wry 0.45 `with_html` and `with_ipc_handler`, JS UI uses `send`; this PUBLIC repository owns `schemas/manifest.schema.json` and `stable/manifest.json` which is `published=false, release=null`.

### Disposition

1. **WebView2 front-end fetch preferred CONDITIONALLY**: verify actual Windows x86 embedded `with_html()` `null`-origin CORS and read full JSON, not just opening the URL. Never use `no-cors`, disabled web security, or anonymous access assumptions. Cap bytes, timeout, response status. Existing IPC reuse only; JS input is untrusted and needs Rust revalidation. WinHTTP via OS may avoid `reqwest`/OpenSSL, but does **not** have literally zero integration/compile/size/maintenance cost and needs redirect/HTTPS/cert/timeouts validation.
2. **Lamp and launcher separation APPROVED**: green only matching reliable installed receipt; red/orange differing release; yellow/gray network/CORS/schema fail; gray installed unknown or `published=false`. All update results advisory. Never override existing game/client identity, security or DLL readiness launch gates.
3. **Buttons APPROVED with label truthfulness**: `btnUpdate` checks unless red, where it is *visibly relabeled* to `下載更新`; `btnDrive` opens a separately operator-approved manual fallback or the validated current published Drive link. If no verified fallback, disable/explain; do not guess a URL. Download is still browser/manual only, not background installation.
4. **Rust URL sample MUST BE REPLACED**: string-prefix matching is not a complete URL security validator. Parse an absolute URL; compare scheme `https`, parsed hostname (never a `host/path` pseudo-host), disallow username/password, non-default port, fragments, controls, encoded path separator/path confusion and unexpected query; match exact approved `drive.google.com/file/d/<ID>/view` (and only explicitly approved query parameters). GitHub metadata fetch must be pinned to exact `raw.githubusercontent.com/<owner>/<repo>/main/stable/manifest.json` URL, separate from approved download-opening hosts; remote JSON does not get to choose GitHub URLs to launch. Reject dangerous redirects or require checking each redirected destination. Open approved URLs through the OS URL-opening API, never `cmd /c start` or interpolate a shell command. Restrict UI IPC command origins and types.
5. **Struct/schema mismatch MUST BE REPAIRED**: public v1 schema requires `schema=1`, `channel=stable`, `published=false => release=null`, and `published=true => release` validated (size>=1, 64 hex sha256, `mandatory=false`). `release_notes` is OPTIONAL in schema, so Rust `String` without default causes valid JSON to fail; use `Option<String>` or `#[serde(default)]`. Rust deserialization should reject unknown fields (`#[serde(deny_unknown_fields)]`) and constrain string lengths/IDs/paths/URL, not just match field types. Rust must explicitly validate semantic cross-field conditions after deserialization. Do not let proposed `mandatory: bool` cause forced update: this public schema explicitly permits only false.
6. **Version/receipt correctness**: `release_id` is an identity, not a safely lexicographically sortable chronology. At minimum equality mismatch => `DIFFERENT_RELEASE` / `UPDATE_AVAILABLE_UNVERIFIED_ORDER`; if "remote greater than local" is required, separately define a strictly monotonic ordinal and downgrade handling. Never infer installed status from last-seen manifest or browser-opening; user acknowledgment remains `USER_ATTESTED` not cryptographic proof.
7. **Regression numbers**: 79 Rust / 85 Python are historical reported counts, not gates. Record actual tests from target-branch HEAD and failures; do not claim executed PASS based on a plan.
8. **Scope control**: no Google API key/GitHub token shipped, no fake release published, no new cloud service, no auto download, eat, game process action or changes to private Rust source in plan-only phase.

## Agent task

```text
TASK_ID=L850-RUST-V03-ARCH-STATIC-REVIEW-20261010
TARGET=Review V0.3 plan against actual public schema and private Wry GUI evidence; author amendments only
KNOWN=public stable/manifest.json published=false; current Wry with_html origin null; current legacy Launcher recovery priority and user updater pause
DO=Reconcile mandatory=false and optional release_notes; specify parsed URL allowlist (exact scheme/host/path and safe redirects), installed-release receipt semantics, CORS test matrix, IPC origin/size limits, non-blocking lamp behavior; record source HEAD and planned tests
PASS=docs-only corrections; no source changes; no fake release; public CURRENT and WORKLOG consistent
STOP=Modify Rust/HTML product source or DLL; publish real or fake ZIP; change published=false; deploy/run game; add API secrets; begin auto download/eat; override existing launcher gates
RETURN=STATUS=PLAN_ONLY|DOCS_REVIEW=PASS|WEBVIEW_CORS=UNTESTED|RUST_URL_VALIDATOR=NEEDS_IMPLEMENTATION|SCHEMA_CONTRACT=RECONCILED|CODE_TOUCHED=NO
```
