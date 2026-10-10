# L850 V0.4 Final Architecture — Conditional PASS with three corrections

```text
DATE=2026-10-10
MODE=PLAN_ONLY
REVIEW_SCOPE=USER_PASTED_V0.4_ARCHITECTURE
STATUS=CONDITIONAL_PASS
PUBLIC_SCHEMA=schemas/manifest.schema.json
PUBLIC_RELEASE=NONE
PUBLIC_MANIFEST_PUBLISHED=FALSE
PRIVATE_RUST_NOTICE_CODE=NOT_IMPLEMENTED
AUTO_DOWNLOAD=PAUSED
AUTO_EAT=PAUSED
IMPLEMENTATION_AUTHORIZATION=NOT_GRANTED
```

## Accepted V0.4 engineering decisions

- Fixed immutable HTTPS public Raw Manifest URL; no GitHub/Google secrets in player Rust EXE.
- WebView2 `fetch()` first *only after Windows x86 CORS/full-body test*, with existing Wry IPC and Rust-side validation; WinHTTP may be the later native fallback if test fails. Windows APIs incur some integration/size/maintenance cost and should not be described as literally free.
- Mirror public schema `schema:1`, `channel:stable`, `published:false -> release:null`, `published:true -> validated ReleasePayload`, `mandatory:false`, `release_notes` optional; reject unknown fields and independently validate string constraints/size/hash.
- Release IDs are exact identifiers, not lexicographic timestamps; dynamically read actual test counts from owning HEAD, zero failures required.
- Update lamp only concerns updates, never overrides existing Launcher client identity/Helper readiness/game security gates. Update check network failure = UNKNOWN, not green.
- Buttons: ordinary check/recheck; RED visibly switches label to Download Update browser link; cloud drive button opens an operator-approved verified public fallback or current approved release URL.

## Required amendments before any coding

### C1 — Path validation must preserve structure and URL semantics

User-supplied V0.4 `split('/').filter(|s| !s.is_empty())` can treat `/file//d/ID/view`, `/file/d/ID//view`, and `/file/d/ID/view/` as equivalent; do not discard empty path segments. Require an exact canonical shape `/file/d/<ID>/view`. Reject percent-encoded ambiguous separators, fragments and unapproved query parameters; forbid userinfo and non-443 ports. Use actual parsed URL `scheme`, host and strict path components with bounded file ID length, and check redirects if any. Manifest URL is a separate fixed fetch constant and must never be chosen by remote payload. Do not invoke `cmd /c start` for external URLs.

### C2 — Missing local installed receipt must not be RED proof

`installed_release_id=None` does not establish that the remote manifest contains a newer release. Return a distinct `InstallStatusUnknown` (neutral gray) and offer the user a manual Drive link. If the installed ID differs, the app may show `DifferentReleaseAvailable` (advisory red/orange) but cannot assert chronological newer without a monotonic `release_sequence`/order contract. No automatic receipt write when a download link is clicked; user attestation must be labeled unverified.

### C3 — Updating never overrides launch prerequisites

In V0.4, green/red/yellow/gray all mean **update status only**. Change the table's `✅ 正常啟動` prose to `UNCHANGED_EXISTING_LAUNCH_GATES`; game launch is conditional on existing client hash, process identity, approved DLL readiness and other owning branch prerequisites. Neither successful update check nor check failure can unlock or block a previously gated launch. The current Rust GUI is an offline prototype; no live launch PASS is claimed.

## Additional contract checks

- Parsed Rust structs do NOT automatically enforce JSON Schema string patterns, `maxLength`, positive size, `mandatory=false` or published/release consistency; implement semantic validation after `serde`. Rust type `u64` itself does not enforce `minimum:1`.
- Ensure IPC message size limit and WebView origin/navigation trust; no arbitrary shell/browser targets; remote string rendering uses `textContent`.
- The public schema currently allows a query suffix in `download_url`; if stricter policy is chosen, either constrain allowed keys in Rust while accepting schema-shaped values, or update both public schema and Rust contract **before implementation**. Schema in PUBLIC repo stays canonical.
- Preserve `published=false, release=null` and never create a fake release or actual ZIP digest.

## Agent task — no source edit

```text
TASK_ID=L850-RUST-V04-PLAN-CONTRACT-CLOSURE-20261010
TARGET=Close C1 URL strict-path rules, C2 missing installed receipt state, C3 launch-gate wording; reconcile public schema and private Rust design
KNOWN=PUBLIC_MANIFEST_PUBLISHED=false; PRIVATE_RUST_NOTICE=NOT_IMPLEMENTED; AUTO_DOWNLOAD=PAUSED; AUTO_EAT=PAUSED
DO=Read public README>CURRENT>this V0.4 adjudication>schema and private Launcher CURRENT; produce final V0.4 contract and offline test matrix without source edits; document actual Windows x86 CORS test as OPEN unless verified
PASS=Three corrections explicit, status matrix unambiguous, semantic validation independent of serde, tests use live HEAD count, docs readback clean
STOP=Do not create update_checker.rs or modify GUI; no `published=true`, no source merge, no executable/test-live/deploy, no updater/eat; do not change old launcher gates
RETURN=HEAD|PLAN_CONTRACT=CONDITIONAL_PASS|C1|C2|C3|SCHEMA_STATUS|WEBVIEW_CORS|IMPLEMENTATION=PAUSED
```
