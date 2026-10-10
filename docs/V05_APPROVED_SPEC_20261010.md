# L850 V0.5 — Approved Design Specification (PLAN ONLY)

```text
DATE=2026-10-10
OWNER=LAUNCHER_HELPER
SPEC_STATUS=APPROVED_WITH_IMPLEMENTATION_NOTES
PHASE=PLAN_ONLY
RUST_SOURCE_CHANGE_AUTHORIZED=NO
PUBLIC_MANIFEST_PUBLISHED=FALSE
AUTO_DOWNLOAD=PAUSED
AUTO_EAT=PAUSED
CORS_WINDOWS_X86=UNTESTED
```

## Accepted final contract

1. **Parsed HTTPS Google Drive URL**: strict `drive.google.com` hostname, no username/password, no fragment, standard HTTPS port, exact canonical `/file/d/<id>/view` five-part path structure including leading empty segment, bounded ID `[A-Za-z0-9_-]{1,128}`, reject repeated/trailing separators and dangerous encoded separators. **Remaining implementation details:** reject raw-path dot-segment normalization ambiguity and percent-encoding tricks, define *exact* allowed query set (default none; alternatively specifically `usp=drive_link`), and verify all redirects before opening/following. Do not rely on string prefix or a broad GitHub domain allowlist. Manifest fetch URL is an immutable exact `raw.githubusercontent.com/8xbwvwd8c9-code-taka/L850-public-updates/main/stable/manifest.json` constant, not remote-configurable.
2. **Rust schema mirror**: `deny_unknown_fields`, `release_notes: Option<String>` or default, `mandatory=false`, numeric positive size, 64-hex digest, bounded lengths and exact release/published invariants. Deserialization alone does not enforce JSON Schema.
3. **Update states**:
   - `published=false,release=null` => NoPublishedRelease (neutral).
   - Published valid release + unknown installed receipt => InstallStatusUnknown (neutral), manual Drive still possible.
   - Published valid release + installed ID == release ID => UpToDate (green).
   - Published valid release + installed ID != release ID => DifferentOfficialRelease (advisory red), not necessarily proven chronologically newer.
   - Network/CORS/invalid manifest => CheckFailed (yellow/gray).
4. **No new game launch gate**: every notice outcome leaves the original client SHA/process/Medium Integrity/child epoch/DLL readiness guards unchanged. This does not imply launcher itself is already live-ready.
5. **Manual buttons**: no automatic ZIP download, install or `eat.exe`. Drive button opens only verified approved static fallback/current manifest URL, and update check button may be *visibly renamed* to Download Update in red. External browser opening must use safe OS URL APIs without `cmd /c start`.
6. **Transport**: existing Wry 0.45/WebView2 `fetch()` + bounded typed existing IPC first, but actual `with_html()` null-origin CORS is OPEN; only after actual Windows x86 test may it be called PASS. Native WinHTTP can be evaluated as a fallback with explicit implementation cost. Revalidate all front-end JSON in Rust. 
7. **Testing**: use the real source HEAD and actual dynamic `cargo test --locked`/Python/Node outputs, not fixed historical test counts. Test path/query/fragment/userinfo/port/encoded ambiguity, published=false, invalid combo, matching/differing/unknown installed ID, malicious JSON/URLs, offline/CORS, no new launch gates.

## Phase plan — not authorizing code

```text
P0=LOCK_PUBLIC_SCHEMA_AND_SPEC
P1=OFFLINE_UPDATE_CHECKER_RS_AND_SECURITY_UNIT_TESTS (WAIT_FOR_USER_IMPLEMENTATION_APPROVAL)
P2=WINDOWS_X86_WEBVIEW2_CORS_TEST (WAIT_FOR_USER_IMPLEMENTATION_APPROVAL)
P3=GUI_IPC_READ_ONLY_NOTICE (WAIT_FOR_USER_IMPLEMENTATION_APPROVAL)
P4=MANUAL_DRIVE_OPEN_BUTTON_BEHAVIOR (WAIT_FOR_USER_IMPLEMENTATION_APPROVAL)
P5=REAL_RELEASE_MANIFEST_PUBLICATION (SEPARATE_APPROVAL_AND_VERIFIED_ZIP_REQUIRED)
```

## Agent task

```text
TASK_ID=L850-RUST-V05-SPEC-HANDOFF-20261010
LANE=LAUNCHER_HELPER
OWNER_PRIVATE_BRANCH=work/850-rust-launcher
PUBLIC_REPO=8xbwvwd8c9-code-taka/L850-public-updates
TARGET=Treat V0.5 as approved design and prepare bounded implementation stages
DO=Read public README > CURRENT > this V0.5 spec > schema > private Launcher CURRENT; inspect existing Wry GUI/IPC, design tests; record open Windows x86 CORS gate and the distinction between release ID inequality versus chronology
PASS=Docs reflect V0.5, public manifest remains disabled, no code or deployment, phase owners/tests scoped
STOP=Do not create update_checker.rs, wire GUI, publish real/fake manifest, enable auto-download/eat, add game launch prerequisites, expose private sources
RETURN=PUBLIC_HEAD|PRIVATE_HEAD|SPEC=V0.5_APPROVED|PHASE=PLAN_ONLY|CORS=OPEN|PUBLIC_RELEASE=NONE|SOURCE_CHANGED=NO
```
